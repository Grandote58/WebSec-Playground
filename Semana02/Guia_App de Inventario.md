# **App de Inventario**



Pasos detallados y el código necesario para construir una aplicación de inventario con **Flet**, **SQLite**, y **Python**.



## 1. Proceso de Instalación de Flet y Configuración del Entorno Virtual en VS Code

### Pasos:

1. **Crear un entorno virtual**:

   - Abre una terminal en tu proyecto.

   - Ejecuta:

     ```bash
     python -m venv env
     ```

   - Activa el entorno virtual:

     - En Windows:

       ```bash
       .\env\Scripts\activate
       ```

     - En Mac/Linux:

       ```bash
       source env/bin/activate
       ```

   

2. **Instalar Flet y SQLite**:

   - Ejecuta:

     ```bash
     pip install flet 
     pip install faker
     ```

     

3. **Configurar Visual Studio Code**:

   - Abre tu proyecto en VS Code.
   - Instala la extensión **Python**.
   - Asegúrate de seleccionar tu entorno virtual:
     - Haz clic en la parte inferior izquierda donde dice "Python Interpreter".
     - Selecciona el entorno virtual que creaste.

------

## 2. Estructura Básica de la Aplicación

### Estructura del Proyecto:

```css
inventario/
│
├── main.py              # Archivo principal que ejecuta la aplicación
├── crud.py              # Archivo con las funciones CRUD
├── db_setup.py          # Script para inicializar la base de datos
├── seed_data.py         # Script para agregar productos de forma aleatoria
└── inventario.db        # Base de datos SQLite (se crea tras ejecutar db_setup.py)
```

------

## 3. Script para Crear la Base de Datos y Tablas

```python
# db_setup.py
import sqlite3

def setup_database():
    conn = sqlite3.connect("inventario.db")
    cursor = conn.cursor()

    # Crear tabla categorías
    cursor.execute("""
    CREATE TABLE IF NOT EXISTS categorias (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nombre TEXT NOT NULL
    )
    """)

    # Crear tabla productos
    cursor.execute("""
    CREATE TABLE IF NOT EXISTS productos (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nombre TEXT NOT NULL,
        descripcion TEXT,
        precio REAL NOT NULL,
        categoria_id INTEGER NOT NULL,
        FOREIGN KEY (categoria_id) REFERENCES categorias(id)
    )
    """)

    # Insertar categorías base
    cursor.executemany("""
    INSERT OR IGNORE INTO categorias (id, nombre)
    VALUES (?, ?)
    """, [
        (1, "Electrónica"),
        (2, "Ropa"),
        (3, "Hogar"),
        (4, "Juguetes"),
        (5, "Libros")
    ])

    conn.commit()
    conn.close()

if __name__ == "__main__":
    setup_database()

```

------

## 4. Script para Agregar Productos de Forma Aleatoria

```python
# seed_data.py
import sqlite3
from faker import Faker
import random

def seed_products():
    fake = Faker()
    conn = sqlite3.connect("inventario.db")
    cursor = conn.cursor()

    for _ in range(30):
        nombre = fake.word().capitalize()
        descripcion = fake.sentence()
        precio = round(random.uniform(10, 1000), 2)
        categoria_id = random.randint(1, 5)

        cursor.execute("""
        INSERT INTO productos (nombre, descripcion, precio, categoria_id)
        VALUES (?, ?, ?, ?)
        """, (nombre, descripcion, precio, categoria_id))

    conn.commit()
    conn.close()

if __name__ == "__main__":
    seed_products()

```

------

## 5. CRUD en SQLite

### Código del CRUD:

```python
# crud.py
import sqlite3

DATABASE = "inventario.db"

def create_product(nombre, descripcion, precio, categoria_id):
    with sqlite3.connect(DATABASE) as conn:
        cursor = conn.cursor()
        cursor.execute("""
        INSERT INTO productos (nombre, descripcion, precio, categoria_id)
        VALUES (?, ?, ?, ?)
        """, (nombre, descripcion, precio, categoria_id))
        conn.commit()

def read_products():
    with sqlite3.connect(DATABASE) as conn:
        cursor = conn.cursor()
        cursor.execute("""
        SELECT p.id, p.nombre, p.descripcion, p.precio, p.categoria_id, c.nombre AS categoria
        FROM productos p
        INNER JOIN categorias c ON p.categoria_id = c.id
        """)
        return cursor.fetchall()

def update_product(product_id, nombre, descripcion, precio, categoria_id):
    with sqlite3.connect(DATABASE) as conn:
        cursor = conn.cursor()
        cursor.execute("""
        UPDATE productos
        SET nombre = ?, descripcion = ?, precio = ?, categoria_id = ?
        WHERE id = ?
        """, (nombre, descripcion, precio, categoria_id, product_id))
        conn.commit()

def delete_product(product_id):
    with sqlite3.connect(DATABASE) as conn:
        cursor = conn.cursor()
        cursor.execute("DELETE FROM productos WHERE id = ?", (product_id,))
        conn.commit()

def get_categories():
    with sqlite3.connect(DATABASE) as conn:
        cursor = conn.cursor()
        cursor.execute("SELECT id, nombre FROM categorias")
        return cursor.fetchall()
```

------



## 6. Interfaz de Usuario con Flet

```python
# main.py
import flet as ft
from crud import create_product, read_products, update_product, delete_product, get_categories

def main(page: ft.Page):
    page.title = "Inventario - CRUD"
    page.scroll = ft.ScrollMode.AUTO

    # Inputs
    nombre = ft.TextField(label="Nombre")
    descripcion = ft.TextField(label="Descripción")
    precio = ft.TextField(label="Precio")
    
    # Dropdown para categorías
    categoria = ft.Dropdown(label="Categoría", options=[
        ft.dropdown.Option(str(cat[0]), cat[1]) for cat in get_categories()
    ])

    # Message display
    message = ft.Text()

    # Botón de actualización (inicialmente oculto)
    save_button = ft.ElevatedButton("Guardar Cambios", visible=False, on_click=None)

    # Table for products
    product_table = ft.DataTable(
        columns=[
            ft.DataColumn(ft.Text("ID")),
            ft.DataColumn(ft.Text("Nombre")),
            ft.DataColumn(ft.Text("Descripción")),
            ft.DataColumn(ft.Text("Precio")),
            ft.DataColumn(ft.Text("Categoría")),
            ft.DataColumn(ft.Text("Acciones"))  # Nueva columna para acciones
        ]
    )

    # Función para refrescar los productos en la tabla
    def refresh_products():
        product_table.rows.clear()
        for product in read_products():
            product_table.rows.append(ft.DataRow(
                cells=[
                    ft.DataCell(ft.Text(str(product[0]))),
                    ft.DataCell(ft.Text(product[1])),
                    ft.DataCell(ft.Text(product[2])),
                    ft.DataCell(ft.Text(f"${product[3]:.2f}")),
                    ft.DataCell(ft.Text(product[4])),
                    ft.DataCell(
                        ft.Row([
                            ft.IconButton(icon=ft.icons.EDIT, on_click=lambda e, product_id=product[0]: load_product_for_edit(product_id)),
                            ft.IconButton(icon=ft.icons.DELETE, on_click=lambda e, product_id=product[0]: delete_product_action(product_id))
                        ])
                    )
                ]
            ))
        page.update()

    # Cargar el producto para edición
    def load_product_for_edit(product_id):
        # Obtener el producto a editar y cargarlo en los campos
        product = next((p for p in read_products() if p[0] == product_id), None)
        if product:
            nombre.value = product[1]
            descripcion.value = product[2]
            precio.value = str(product[3])
            categoria.value = str(product[4])

            # Mostrar el botón "Guardar Cambios" y configurar el evento on_click
            save_button.visible = True
            save_button.on_click = lambda e, pid=product_id: save_changes(pid)

            message.value = f"Editando producto ID: {product_id}"
            page.update()

    # Guardar cambios de un producto editado
    def save_changes(product_id):
        update_product(product_id, nombre.value, descripcion.value, float(precio.value), int(categoria.value))
        
        # Mostrar ventana emergente de confirmación
        confirmation_dialog = ft.AlertDialog(
            title=ft.Text("Éxito"),
            content=ft.Text("La información se actualizó correctamente."),
            actions=[ft.TextButton("Cerrar", on_click=close_confirmation_dialog)]
        )
        page.dialog = confirmation_dialog
        confirmation_dialog.open = True
        page.update()

    # Cerrar ventana de confirmación y ocultar el botón "Guardar Cambios"
    def close_confirmation_dialog(e):
        page.dialog.open = False
        save_button.visible = False  # Ocultar el botón "Guardar Cambios"
        clear_form()
        refresh_products()
        page.update()

    # Agregar un nuevo producto
    def add_product(e):
        if categoria.value:  # Validar que se haya seleccionado una categoría
            create_product(nombre.value, descripcion.value, float(precio.value), int(categoria.value))
            message.value = "Producto agregado exitosamente."
            clear_form()
            refresh_products()
        else:
            message.value = "Por favor, seleccione una categoría."
        page.update()

    # Borrar un producto
    def delete_product_action(product_id):
        delete_product(product_id)
        message.value = "Producto eliminado exitosamente."
        refresh_products()
        page.update()

    # Limpiar el formulario
    def clear_form():
        nombre.value = ""
        descripcion.value = ""
        precio.value = ""
        categoria.value = ""
        page.update()

    # Agregar componentes a la página
    page.add(
        nombre, descripcion, precio, categoria,
        ft.ElevatedButton("Agregar Producto", on_click=add_product),
        save_button,  # Botón "Guardar Cambios" agregado aquí
        product_table,
        message
    )

    refresh_products()

ft.app(target=main)

```

------

## 7. Instrucciones de Ejecución

1. **Configura la base de datos**:

   ```bash
   python db_setup.py
   ```

2. **Agrega productos iniciales**:

   ```bash
   python seed_data.py
   ```

3. **Ejecuta la aplicación**:

   ```bash
   python main.py
   ```

4. **Abre la aplicación en el navegador**: Al ejecutar, Flet abrirá automáticamente la aplicación en tu navegador.





