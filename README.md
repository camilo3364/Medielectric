# Medielectric

##### Medidor electrónico
Este proyecto consiste en la medición de la red eléctrica por medio de un sensor sct no invasivo de corriente eléctrica y 
un transformador de voltaje para poder reducir la señal, además de un circuito acondicionador para que el adc(conversor 
analogo digital) pueda procesar esta información sin daño alguno. Esto para cargas resistivas es innato, pero para cargas
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
   
       Arduino=serial.Serial("/dev/ttyACM0",baudrate=9600,timeout=10)   `
      
   Luego de eso creamos el main en python y declaramos variables que posteriormente vamos a usar:  
      
      if __name__ =="__main__":
	sumatoria=0.0
	sumatoria1=0.0
	sumatoria2=0.0
	sumatoria3=0.0
	bandera=0.0
	bandera1=0.0
	bandera2=0.0
	bandera3=0.0
	bandera4=0.0
	corriente_negativa=0
	potencia_promedio=0
	voltaje_negativo=0
	bandera5=0.0
	bandera6=0.0
	bandera7=0.0
	bandera8=0.0
	sumatoria5=0.0
	sumatoria6=0.0
	sumatoria7=0.0
	sumatoria8=0.0
	contador_x=0.0
	potencia1=0.0
	potencia2=0.0
	potencia3=0.0
	potencia4=0.0
	potencia5=0.0
	potencia6=0.0
	potencia7=0.0
	potencia8=0.0
	potencia9=0.0
	potencia10=0.0
	potencia_filtrada=0.0
	potencia_actual=0.0
	potencia_anterior=0.0
	contadornn=0.0
	cosenofi=0.0
	a=0
	c=0
	f=0
	j=0
	bandera_potencia_hora=0
	potencia_hora=0
	potencia_kwh=0
 
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

###### Ahora que si se posee la información se debe visualizar y la manera en que se efectua esto es mediante la facturación enviando un correo electrónico cada mes y enviando datos a ThingSpeak para mantener un control en todo momento de los datos.

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


-Para subir los datos a ThingSpeak está el siguiente fragmentno de código 

			val=potencia_filtrada                      ##En esta parte generamos
			URL='https://api.thingspeak.com/update?api_key='
			KEY='2OJ8ZCPIMOGEPIBT'                  ##key que genera el thingspeak
			HEADER='&field1={}'.format(val) ##carga las variables al$
			new_URL=URL+KEY+HEADER          ##genera la direccion url
			print(new_URL)                  #muestra el proceso de subir datos en c$
			datos=urllib.request.urlopen(new_URL)
			print(datos)   #carga a thinspeak los datos
			
# Recursos

-Link de la página en dónde muestran el funcionamiento del sensor sct
https://programarfacil.com/blog/arduino-blog/sct-013-consumo-electrico-arduino/

-Link del video del proyecto 


# Construido con

-Atom y Arduino

# Código completo!

`#Librerias`

`import serial,math,smtplib,time,threading,itertools,urllib.request`

`import os.path as op`

`from datetime import date,timedelta,datetime`

`from email.mime.multipart import MIMEMultipart`

`from email.mime.text import MIMEText`

`from email.mime.image import MIMEImage`

`from email.mime.base import MIMEBase`

`from email import encoders`

`from time import time,clock`

`from reportlab.lib import colors`

`from reportlab.lib.pagesizes import letter`

`from reportlab.platypus import SimpleDocTemplate, Table, TableStyle`

`from reportlab.pdfgen import canvas`

`from reportlab.lib.pagesizes import letter`

`from reportlab.lib.units import inch`

`from reportlab.lib.colors import pink, green, brown, white`


`#Serial`

`Arduino=serial.Serial("/dev/ttyACM1",baudrate=9600,timeout=10)`

`#Main

`if __name__ =="__main__":`

	#Variables Corriente
	sumatoria=0
	sumatoria1=0
	sumatoria2=0
	sumatoria3=0
	bandera=0
	bandera1=0
	bandera2=0
	bandera3=0
	corriente_negativa=0

	#Variables Voltaje
	sumatoria4=0
	sumatoria5=0
	sumatoria6=0
	sumatoria7=0
	bandera4=0
	bandera5=0
	bandera6=0
	bandera7=0
	voltaje_negativo=0

	#Variables Potencia
	potencia1=0
	potencia2=0
	potencia3=0
	potencia4=0
	potencia5=0
	potencia6=0
	potencia7=0
	potencia8=0
	potencia9=0
	potencia10=0
	potencia_filtrada=0
	bandera_potencia_hora=0
	potencia_hora=0
	potencia_kwh=0
	potencia_actual=0
	potencia_anterior=0

	#Otras Constantes
	cont=0
	b=0
	d=0
	f=0

	while(True):

		##Lectura Arduino##
		data= str(Arduino.readline())
		separado= str.split(data,":")
		separado_2= str.split(separado[0],"'")
		separado_3= str.split(separado[1],"\\")
		separado_4= str.split(separado[2],"\\")
		A= float(separado_2[1])
		B= float(separado_3[0])
		C= float(separado_4[0])


		##Factor de potencia##
		cosenofi= math.cos(C)
		cosenofi= round(cosenofi,3)
		cosenofi= abs(cosenofi)


		##Corriente##
		data1= A - 2.5
		corriente= data1*12
		if(corriente_negativa<corriente):
			corriente_negativa= corriente
			bandera= bandera + 1
			cont= cont + 1
			sumatoria= sumatoria + (corriente_negativa**2)
		elif(corriente_negativa>corriente):
			corriente_negativa= corriente
			bandera1= bandera1 + 1
			sumatoria1=sumatoria1 + (corriente_negativa**2)
		elif(corriente_negativa<corriente):
			corriente_negativa= corriente
			bandera2= bandera2 + 1
			sumatoria2= sumatoria2 + (corriente_negativa**2)
		elif(corriente_negativa>corriente):
			corriente_negativa= corriente
			bandera3= bandera3 + 1
			sumatoria3= sumatoria3 + (corriente_negativa**2)
		elif(corriente==0):
			corriente=0
			sumatoria=0
			sumatoria1=0
			sumatoria2=0
			sumatoria3=0

		if(bandera!=0 or bandera1!=0 or bandera2!=0 or bandera3!=0):
			corriente= sumatoria + sumatoria1
			banderax= bandera + bandera1 + bandera2 + bandera3
			corriente= math.sqrt(2*corriente / banderax)
			corriente= round(corriente,3)


		##Voltaje##
		data2= B - 2.5
		voltaje= data2*(115/12)*(24/5)
		if(voltaje_negativo<voltaje):
			voltaje_negativo= voltaje
			bandera4= bandera4 + 1
			sumatoria4= sumatoria4 + (voltaje_negativo**2)
		elif(voltaje_negativo>voltaje):
			voltaje_negativo= voltaje
			bandera5= bandera5 + 1
			sumatoria5= sumatoria5 + (voltaje_negativo**2)
		elif(voltaje_negativo<voltaje):
			voltaje_negativo= voltaje
			bandera6= bandera6 + 1
			sumatoria6= sumatoria6 + (voltaje_negativo**2)
		elif(voltaje_negativo>voltaje):
			voltaje_negativo= voltaje
			bandera7= bandera7 + 1
			sumatoria7= sumatoria7 + (voltaje_negativo**2)
		elif(voltaje==0):
			voltaje=0
			sumatoria4=0
			sumatoria5=0
			sumatoria6=0
			sumatoria7=0

		if(bandera4!=0 or bandera5!=0 or bandera6!=0 or bandera7!=0):
			voltaje= sumatoria4 + sumatoria5
			banderay= bandera4 + bandera5 + bandera6 + bandera7
			voltaje= math.sqrt(2*voltaje / banderay)
			voltaje= round(voltaje,3)


		##Inicio Calculo potencia##

		#Potencia Promedio
		potencia= corriente*voltaje

		potencia10=potencia9
		potencia9=potencia8
		potencia8=potencia7
		potencia7=potencia6
		potencia6=potencia5
		potencia5=potencia4
		potencia4=potencia3
		potencia3=potencia2
		potencia2=potencia1
		potencia1=potencia

		potencia_filtrada= (potencia + potencia1 + potencia2 + potencia3 + potencia4 + potencia5 + potencia6 + potencia7 + potencia8 + potencia9 + potencia10) / 11
		potencia_filtrada= round(potencia_filtrada,3)

		bandera_potencia_hora= bandera_potencia_hora + 1
		potencia_hora= potencia_hora + potencia_filtrada
		potencia_hora= potencia_hora / bandera_potencia_hora

		#Potencia Hora
		a=clock()
		if(a-b > 1):#60
			potencia_kwh= (potencia_hora*1)/1000
			potencia_kwh= round(potencia_kwh,3)
			b=a

		potencia_anterior= potencia_actual
		potencia_actual= potencia_kwh

		#Impresion Potencia
		if(cont==30):
			print("I:     "+"V:     "+"P:     ")
			print(corriente , voltaje , potencia_filtrada)
			cont=0


		##Correo##
		c=clock()
		if(c-d > 30):#180
			#Estructura
			anfitrion = "smtp.gmail.com"
			puerto = 587
			direccionDe = "2420171047@estudiantesunibague.edu.co"
			contrasenaDe = "camilo900613"
			direccionPara = "2420171040@estudiantesunibague.edu.co"
			servidor = smtplib.SMTP(anfitrion,puerto)
			servidor.starttls()
			servidor.login(direccionDe,contrasenaDe)
			print("")
			print("Enviando Factura...")
			print("")
			asunto = "Medielectric"
			correo = MIMEMultipart()
			correo['From'] = direccionDe
			correo['To'] = direccionPara
			correo['Subject'] = asunto
			#Logo
			file = open("logo.png", "rb")
			attach_image = MIMEImage(file.read())
			attach_image.add_header('Content-Disposition', 'attachment; filename = "logo.png"')
			correo.attach(attach_image)
			#Mensaje
			message = """Estimado usuario, Juan Camilo Buitrago Diaz
	Como está acordado en el contrato de mensualidad de pago se le anexa el recibo eléctrico para su cancelación.

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

			mensaje = MIMEText(message)
			correo.attach(mensaje)
			servidor.sendmail(direccionDe,direccionPara,correo.as_string())
			d=c


		##ThingSpeak##
		e=clock()
		if(e-f > 10):#90
			val=potencia_filtrada
			val1=corriente
			val2=voltaje
			URL='https://api.thingspeak.com/update?api_key='
			KEY='R13A4I1DK051F8TV'
			HEADER='&field1={}&field2={}&field3={}'.format(val,val1,val2)
			new_URL=URL+KEY+HEADER
			print("")
			print("Subiendo Informormacion al ThingSpeak...")
			print("")
			datos=urllib.request.urlopen(new_URL)
			f=e


		##Documento pdf##
		Titulo= 'Recibo Eléctrico'
		#potencia_actual=potencia_kwh
		#potencia_anterior=potencia_anterior
		factor_potencia=cosenofi
		c=canvas.Canvas("reciboPdf_tablas_si.pdf")
		hoy=date.today()
		mes_actual=hoy.month
		mes_pasado=hoy-timedelta(days=31)
		mes_pasado=mes_pasado.month
		valor_kwh=550

		#Ancho de linea
		c.setLineWidth(.3)

		#Fuente y tamano
		c.setFont('Helvetica',14)

		#Dibuja texto en posicion X y Y por puntos. 1pt= 1/72 pulgadas
		c.drawString(400,630,"Fecha:  " + str(hoy))
		c.drawString(100,630,"Titular:    Juan Camilo Buitrago Diaz ")
		c.drawString(100,600,"Dirección:  Avenida Ferrocarril")
		c.drawString(100,570,"Manzana 29, Casa 16")
		c.drawString(195,492,"Medición de consumo")
		c.drawString(148,472,"Mes")
		c.drawString(150,452,str(mes_actual))
		c.drawString(150,432,str(mes_pasado))
		c.drawString(262,472,"Kwh")
		c.drawString(262,452,str(potencia_filtrada))
		c.drawString(262,432,str(potencia_anterior))
		c.drawString(360,472,"Valor a pagar")
		c.drawString(378,452,"$"+str(round(valor_kwh*potencia_filtrada,2)))
		c.drawString(378,432,"$"+str(round(valor_kwh*potencia_anterior,2)))
		c.drawString(220,300,"Desplazamiento de fase ",2)
		c.drawString(285,270,str(factor_potencia))
		c.drawString(104,402,"Valor Kwh")
		c.drawString(262,402,"$"+str(valor_kwh))
		#Posicion X1 Y1 X2 Y2
		c.line(150,628,330,628)
		c.line(167,598,297,598)
		c.line(450,628,525,628)

		#Imagen
		c.drawImage("logo.png", 420, 725,width=150, height=100)
		c.drawImage("logo2.png", 450, 415,width=150, height=100)
		c.drawImage("frame.png", 210, 30,width=180, height=180)

		#Tabla
		c.setFillColorRGB(255, 0,0 )
		c.setFont("Courier-Bold", 25)
		c.drawCentredString(290,700, Titulo)

		#Rectangulo (cx,cy,ancho,alto)
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
		c.rect(100,400,120,20)
		c.rect(220,400,120,20)

		#Circulo (cx,cy,ancho,alto)
		c.circle(300,300,90,1)

		#Escribe y guarda el documento
		c.showPage()
		c.save()

# Autores
Universidad de Ibagué -Ingeniería Electrónica
- **Juan Pablo Siachica**
- **Juan Camilo Buitrago**
