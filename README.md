# XLMRat
# Como recuperar el hash del reto XLMRat de ciberdefenders

1. En el wireshark vamos a **`file`** buscamos **`Export Objects`** escogemos el protocolo en este caso **`HTTP`** selecionamos el archivo a descargar para este ejecicio es **`mdm.jpg`** confirmamos con **`save`**
2. El archivo **`mdm.jpg`** contiene texto con el hex codificado, puedes extraerlo directamente así:
```bash
cat mdm.jpg | tr -cd '0-9A-Fa-f_' > hex.txt
```
3. Creamos una funcion en python **`extractor.py`**
```bash
# Leer el archivo que contiene el hex
with open("hex.txt", "r") as file:
    hex_string = file.read()

# Limpiar: eliminar guiones bajos, espacios, saltos de línea y cualquier otro carácter no hexadecimal
hex_string = hex_string.replace('_', '').replace(' ', '').replace('\n', '').replace('\r', '')

# Validar si la longitud es par (cada byte son 2 caracteres)
if len(hex_string) % 2 != 0:
    raise ValueError("La longitud de la cadena hexadecimal no es par. Puede faltar un carácter o estar corrupto.")

# Convertir la cadena hexadecimal a datos binarios
bin_data = bytes.fromhex(hex_string)

# Guardar los datos binarios como archivo ejecutable
with open("recuperado.exe", "wb") as f:
    f.write(bin_data)

print("[✔] Archivo recuperado exitosamente como 'recuperado.exe'")
```

4. Ejecutamos **`python3 extractor.py`**
   
```bash
python3 extractor.py
```

5. Resulatdo esperado
 ```bash
 [✔] Archivo recuperado exitosamente como 'recuperado.exe'
 ```

 6. Ejecutar
  ```bash
    file recuperado.exe
    sha256sum recuperado.exe
    ls -lh recuperado.exe
  ```

  Si todo salio bien deberias estar viendo el ejecutable, hash y permisos
  
   

