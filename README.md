# Standard File Manager

Desktop application for creating and managing data files made of **fixed-length records**, with a **B-tree index** on the primary key. Built in Java (Swing) for the Data Structures II course at UNITEC (2024).

The point of the project is what happens underneath the UI: how records are laid out on disk, how deleted space gets reused, and how an index keeps lookups fast without reading the whole file.

## How it works

- **User-defined schema.** Each file starts with a header that describes its fields (name, data type, length, and which one is the primary key). Records are then written with that fixed layout, so any record can be reached by offset with `RandomAccessFile`.
- **Avail list.** Deleting a record marks its slot as free and stores it in an availability list. The next insert reuses that slot instead of growing the file.
- **B-tree index (order 6).** Primary keys are kept in a B-tree that maps each key to the record's byte offset in the file. It supports search, insert with node splitting, and delete, and it's persisted to its own `.Tree` file with Java serialization.
- **Secondary indexes.** You can build an index on another field and re-index the file.

## Features

- Create, open, save and close data files
- Create, list, modify and delete fields (fields are locked once the file has records)
- Create, search, list, modify and delete records
- Primary-key lookups through the B-tree
- Create secondary indexes and re-index files

Not implemented: cross-joining two files, and exporting to Excel or XML (the buttons exist in the UI but have no logic yet).

## Project structure

```
src/standardfilemanager/
├── Main.java        # Swing UI and file/record operations
├── BTree.java       # B-tree: search, insert, split, delete
├── Nodo.java        # B-tree node
├── Key.java         # Key + byte offset of the record
├── AdminBTree.java  # Loads and saves the tree to disk
└── Campo.java       # Field definition (name, type, length, key)
```

## Running it

Requires JDK 22 or newer.

```bash
java -jar dist/StandardFileManager.jar
```

Or open the folder as a NetBeans project and run it.

---

<details>
<summary><b>En español</b></summary>

Aplicación de escritorio en Java (Swing) para crear y administrar archivos de **registros de longitud fija**, con un **índice en árbol B** sobre la llave primaria. Proyecto de Estructuras de Datos II en UNITEC (2024).

- Cada archivo empieza con un encabezado que describe sus campos; los registros se leen y escriben por desplazamiento con `RandomAccessFile`.
- Al borrar un registro, su espacio entra a una **avail list** y se reutiliza en la siguiente inserción.
- Las llaves primarias se guardan en un **árbol B de orden 6** (búsqueda, inserción con división de nodos y borrado), persistido en un archivo `.Tree`.
- Permite crear índices secundarios y reindexar.

Pendiente: cruzar archivos y exportar a Excel o XML.

</details>
