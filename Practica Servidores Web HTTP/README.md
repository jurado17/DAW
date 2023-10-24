#  SERVIDOR WEB HTTP
### 1. Instalación de Python
Comprobamos que tenemos instalado Python en nuestro ordenador. Para ello accedemos al <b>Simbolo del sistema</b> y escribimos "python". Si ya lo tenemos instalado nos aparecerá la versión que tenemos 
instalada como en la siguiente imagen.

<image src="https://github.com/jurado17/DAW/blob/main/Practica%20Servidores%20Web%20HTTP/img/Captura%20de%20pantalla%202023-10-20%20104112.png">

Si no, accedemos a la pagina web de pagina web de Python para instalarlo.

### 2. Prueba de los Servidores

Ahora vamos a probar diferentes codigos python en nuestro servidor para iniciarlo.

Probamos el primer codigo:
```python
import http.server as httpserver
import socketserver

def main(port=None):
	if port is None:
		port = 8000
	handler = httpserver.SimpleHTTPRequestHandler
	try:
		httpd = socketserver.TCPServer(("", port), handler)
		print("serving at port", port)
		httpd.serve_forever()
	except OSError:
		print("Given PORT:{} is unavailable.Try running with diffrent PORT Number!".format(port))

if __name__ == '__main__':
	main()
```
Al compilar este codigo nos saldra esto:
<image src="https://github.com/jurado17/DAW/blob/main/Practica%20Servidores%20Web%20HTTP/img/ejemplo%201.png">
