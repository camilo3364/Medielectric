# Medielectric

##Medidor electrónico
Este proyecto consiste en la medición de la red eléctrica por medio de un sensor sct no invasivo de corriente eléctrica y 
un transformador de voltaje para poder reducir la señal, además de un circuito acondicionador para que el adc(conversor 
analogo digital) pueda procesar esta información sin daño alguno. Esto para cargar resistivas es innato, pero para cargas
reactivas como un motor no se puede calcular tan fácil, entonces se recurre a encontrar el desfase que se génera entre la
corriente y el voltaje, para ello se creó un circuito electrónico para detectar el cruce por cero de ambas señales y ayudados con
una compuerta Xor comparar cuando ambas señales fueran diferentes, siendo que cuando fueran diferentes ambas señales, la compuerta
se pondría en alto el tiempo del desfase, entoncces con regla de 3 sabiendo que la señal que se mide es la red eléctrica y en
Colombia es de 60Hz y eso traduce a que el periodo es de 16.6ms apróximados y eso equivale a 360 grados. Después de encargarse el
programa de procesar la información de la potencia enviará el consumo eléctrico mediante un correo electrónico subiendo y creando el
mismo un pdf con dichas instrucciones, además de subir datos a Thingspeak para poder monitorear en tiempo real el consumo energético
que se tenga.

# Pre-requisitos
Cómo pre requisitos se necesitan los siguientes componentes y programas:

-Circuito acondicionador (tanto de voltaje como de corriente)

-Circuito cruce por cero (esto si se desea calcular la potencia de cargas reactivas)

-ADC(Conversor analogo digital) en este caso se usó Arduino

-Ubuntu 18.04

-Python 3

-Spyder o Atom como editores, también se puede usar la terminal para esto.

después de esto se necesitan las siguientes librerías: 

-Libreria serial
-Libreria math
-Libreria smtplib
-Libreria time
-Libreria utllib.request
-Libreria os.path
-Libreria reportlab

# Instalación

Para la instalación abrimos la terminal (Ctrl+Alt+T) y escribimos los siguientes comandos

  `sudo apt-get install python3`

lo instalan y ya tendrán python3, para descargar e instalar spyder y atom se sigue el mismo proceso

   `sudo apt-get install atom`
   
con ese comando se instalará el editor de atom para python y para el editor spyder es

`sudo apt-get install spyder`

Después de haber instalado python y alguno de los dos editores de python se procede a instalar las librerías tanto Serial como el resto
para comenzar con la lectura de datos y procesamiento de los mismos:

  `sudo apt-get install python-time`
  
  `sudo apt-get install python-smtplib`
  
  `sudo apt-get install python-reportlab`
  
  `sudo apt-get install python-os.path`
  
  `sudo apt-get install python-urllib.request`
  
   # Código del ADC(Conversor) 
   Ya que se usó arduino para el procesamiento de las señales, dejaré el codigo aquí debajo:
   
  `double A=0;`
  `double C=0;`
  `double B=0;`
  `double D=0;`
  `String voltaje, corriente,fimetro;`
  `double tic,toc,delta,angulo;`
  `int pin=8;`
  `int value=0;`
  `float f1,f2,f3,f4,f5,f6,f7,f8,f9,f10,f11,f12,f13,f14,delta1;`

  `void setup()`
  `{`
   `Serial.begin(9600);`
   `pinMode(pin,INPUT);`
  `}`
 
  `void loop()` 
  `{  `
   `value=digitalRead(pin);`
   `A= analogRead(A0);   // realizar la lectura`
   `C= analogRead(A1);   // realizar la lectura`                      
   `B= (A*(5.00))/1023.00;`
   `D= (C*(5.00))/1023.00;`
   `voltaje=String(D);`
   `corriente=String(B);`
   `tic= millis();`
   `if(value==HIGH)`
   `{ //Serial.println("Encendido");`
    ` toc=millis();`
     `}`
   `delta=toc-tic;`
   `angulo=(delta*(360*60))/1000;`
   `fimetro=String(angulo);`

   `Serial.println(corriente+":"+voltaje+":"+fimetro);`
   `delay(33.34);`
  `}`
  
   # Ejecutando las pruebas 
   Primero entramos al editor de python Spyder, esto puede ser desde la terminal (Ctrl+T) o abriendo el programa.
   Después de ya estar en el editor Spyder se procede a importar las librerías de la siguiente manera :
   
   ` import serial,math,smtplib,time,os.path, urllib.request`
   
   Después de haber importado las librerías se crea una variable en la que se va a almacenar los datos del Arduino en python
   
      ` Arduino=serial.Serial("/dev/ttyACM0",baudrate=9600,timeout=10)   `
      
      Luego de eso creamos el main en python y declaramos variables que posteriormente vamos a usar:  
      
      `if __name__ =="__main__":`
	`sumatoria=0.0`
	`sumatoria1=0.0`
	`sumatoria2=0.0`
	`sumatoria3=0.0`
	`bandera=0.0`
	`bandera1=0.0`
	`bandera2=0.0`
	`bandera3=0.0`
	`bandera4=0.0`
	`corriente_negativa=0`
	`potencia_promedio=0`
	`voltaje_negativo=0`
	`bandera5=0.0`
	`bandera6=0.0`
	`bandera7=0.0`
	`bandera8=0.0`
	`sumatoria5=0.0`
	`sumatoria6=0.0`
	`sumatoria7=0.0`
	`sumatoria8=0.0`
	`contador_x=0.0`
	`potencia1=0.0`
	`potencia2=0.0`
	`potencia3=0.0`
	`potencia4=0.0`
	`potencia5=0.0`
	`potencia6=0.0`
	`potencia7=0.0`
	`potencia8=0.0`
	`potencia9=0.0`
	`potencia10=0.0`
	`potencia_filtrada=0.0`
	`potencia_actual=0.0`
	`potencia_anterior=0.0`
	`contadornn=0.0`
	`cosenofi=0.0`
	`a=0`
	`c=0`
	`f=0`
	`j=0`
	`bandera_potencia_hora=0`
	`potencia_hora=0`
	`potencia_kwh=0`
 
 Cada una de estas constantes serán usadas al avanzar en el codigo.
 
 -Ahora para poder leer en python lo que nos llega de forma serial del arduino se hace lo siguiente, ya que la 
 información se está enviando como una cadena de texto, entonces se necesita decodificar esa información y por
 ende separarla y cambiar de cadena de caracteres a numeros que es lo que se necesita.
 
 `data=str(Arduino.readline())`
 `separado=str.split(data,":")`
 `separado_2=str.split(separado[0],"'")`
 `separado_3=str.split(separado[1],"\\")`
 `separado_4=str.split(separado[2],"\\")`
 `A=float(separado_2[1])`
 `B=float(separado_3[0])`
 `C=float(separado_4[0])`
 `cosenofi=math.cos(C)`
 `cosenofi=round(cosenofi,3)`
 `cosenofi=abs(cosenofi)`
 
 Lo que se efectua en esas variables separado es mediante la funcuón split separar la cadena de caracteres que llega 
 de arduino justo en dónde se haya colocado una (') o (\\), esto para separar los datos de lectura, tanto de corriente,
 voltaje y el angulo de desfase.
 En las variables A,B,C,D se separa la información y de caracter String se transforma a float(número). Para obtener 
 el desplazamiento es el coseno del ángulo de desfase.
 
 -Lo siguiente será calcular el valor eficaz tanto de la corriente como el voltaje. Para ello se usa la ecuacuón 
 en tiempo discreto. Esto se explica más detallado en el informe.
 
 La ecuación es: 
 
![ecuacion](https://user-images.githubusercontent.com/55809543/69359916-8d44b600-0c57-11ea-97d0-59e609904ee0.jpeg)

Está ecuación se implementa tanto para el voltaje como para la corriente.

-Después de obtener ambos valores RMS se procede a obtener la potencia, puesto que potencia=corriente*voltaje, pero para obtener la potencia para cualqueir carga se debe multiplicar por el coseno del ángulo de desfase. Aunque eso no es todo, porque ese valor de potencia no es más que el valor instantaneo, o sea en esa fracción de momento y lo que se necesita enviar al usuario es la energia consumida en kwh, para esto se usa la siguiente conversión:

![ecuacion2](https://user-images.githubusercontent.com/55809543/69360913-6d15f680-0c59-11ea-8657-8f03a8f22a97.png)

## Ahora que si se posee la información se debe visualizar y la manera en que se efectua esto es mediante la facturación enviando un correo electrónico cada mes y enviando datos a ThingSpeak para mantener un control en todo momento de los datos.

Para esto se usan las librerias smtpelib, reportlab y urllib.request, además de que se necesita un tiempo especifico para enviar cada uno de esto, para ello se usa la libreria time y se importa la función clock.

-Comenzaremos con el correo:

			anfitrion = "smtp.gmail.com"
			puerto = 587
			direccionDe = "2420171047@estudiantesunibague.edu.co"
			contrasenaDe = "5555555"
			direccionPara = "2420171047@estudiantesunibague.edu.co" # PAra quien
			servidor = smtplib.SMTP(anfitrion,puerto)
			servidor.starttls()
			servidor.login(direccionDe,contrasenaDe)
			print(servidor.ehlo())
			asunto = "Medielectric"
			correo = MIMEMultipart() # SE CREA EL OBJETO
			correo['From'] = direccionDe
			correo['To'] = direccionPara
			correo['Subject'] = asunto
			p=str(potencia_filtrada)
			#logo
			file = open("logo.png", "rb")
			attach_image = MIMEImage(file.read())
			attach_image.add_header('Content-Disposition', 'attachment; filename = "logo.png"')
			correo.attach(attach_image)
			#FIn logo
			message = """Estimado usuario, Juan Camilo Buitrago Diaz
como está acordado en el contrato de mensualidad de pago se le anexa el recibo eléctrico para su cancelación.

Su fiel y eficiente servicio Medielectric estará atento y dipuesto a servirlo en lo que podamos.

Número de contacto #1: 272 56 47
Número de contacto #2: 312 313 02 07

                                        Fan page:
								www.facebook.com/Medielectric
							  	   www.Medielectric.com
 """
			#Archivo adjundo
			part = MIMEBase('application', "octet-stream")
			part.set_payload(open("reciboPdf_tablas_si.pdf", "rb").read())
			encoders.encode_base64(part)
			part.add_header('Content-Disposition',
                        'attachment; filename="Recibo electrico"')
			correo.attach(part)
			#fin de adjuntar

			mensaje = MIMEText(message)
			correo.attach(mensaje)
			servidor.sendmail(direccionDe,direccionPara,correo.as_string())
			
			
			
			
Como se puede observar el código se escribio de tal forma que fuera muy intuitivo saber que colocar y en donde colocarlo, el archivo adjunto no es más que el logo del proyecto y además el pdf que se va a crear más adelante. Cabe recordar que se necesita crear una carpeta en donde esté todo ahí mismo, el logo y el pdf para evitar errores del programa.

-Para el PDF va lo siguiente: 

		Titulo= 'Recibo Eléctrico'
		potencia_actual=potencia_kwh
		potencia_anterior=potencia_anterior
		factor_potencia=cosenofi
		c=canvas.Canvas("reciboPdf_tablas_si.pdf")
		dt= datetime.now()
		#ancho de linea
		c.setLineWidth(.3)
		#drawMyRuler(c)
		#fuente y tampano
		c.setFont('Helvetica',14)
		#dibuja texto en pos X e Y por puntos. 1pt= 1/72 pulgadas
		c.drawString(468,630,"Fecha:  " + str(dt))
		c.drawString(100,630,"Titular:    Juan Camilo Buitrago Diaz ")
		#c.drawString(130,700,"Juan Camilo Buitrago Diaz")
		c.drawString(100,600,"Dirección:  Avenida Ferrocarril")
		c.drawString(100,570,"Manzana 29, casa 16")
		c.drawString(195,492,"Medición de consumo")
		c.drawString(262,472,"Mes")
		c.drawString(256,452,"Actual")
		c.drawString(252,432,"Pasado")
		c.drawString(378,472,"Kwh")
		c.drawString(102,452,"Medida")
		c.drawString(102,432,"Medida")
		c.drawString(245,300,"Factor de potencia",2)
		c.drawString(285,270,str(factor_potencia))
		c.drawString(378,452,str(potencia_actual))
		c.drawString(378,432,str(potencia_anterior))
		#c.drawString(100,660,"Casa 21 ")

		#pos x1 y1 x2 y2
		c.line(150,628,330,628)
		c.line(167,598,297,598)
		c.line(520,628,590,628)
		#para colocar una imagen
		c.drawImage("logo.png", 420, 725,width=150, height=100)
		c.drawImage("logo2.png", 450, 415,width=150, height=100)
		c.drawImage("codigo.png", 220, 50,width=150, height=100)
		#tablas
		c.setFillColorRGB(255, 0,0 )
		c.setFont("Courier-Bold", 25)
		c.drawCentredString(290,700, Titulo)

		#cx=50
		#cy=600
		#ancho=100
		#alto=30
		#c.rect(cx,cy,ancho,alto)
		c.rect(100,490,360,20)
		c.rect(100,470,120,20)
		c.rect(220,470,120,20)
		c.rect(340,470,120,20)
		c.rect(100,450,120,20)
		c.rect(220,450,120,20)
		c.rect(340,450,120,20)
		c.rect(100,430,120,20)
		c.rect(220,430,120,20)
		c.rect(340,430,120,20)

		c.circle(300,300,80,1)

		# write the document to disk
		c.showPage()
		c.save()
			
      
   Lo que esto hace es generar un pdf el cual se vera en la siguiente imagen: 
   
   
![Captura de pantalla de 2019-11-21 12-32-36](https://user-images.githubusercontent.com/55809543/69361761-1f9a8900-0c5b-11ea-8454-9dd0eb4825d1.png)
