# Sintaxis y Semantica de los Lenguajes

* **Curso:** K2051
* **Año de cursada:** 2018
* **Legajo:** 167573-4
* **Apellido:** Castro
* **Nombre:** Santiago Matias

## Trabajo 01

### Enunciado

#### Tareas
1. Inverstigar las funcionalidades y opciones que su compilador presenta para limitar el inicio y fin de las fases de traducción.
2. Para la siguiente secuencia de pasos:
	* Transcribir en readme.md cada comando ejecutado.
	* Describir en readme.md el resultado u error obtenidos para cada paso.

#### Secuencia de pasos

1. Escribir hello2.c, que es una variante de hello.c:
*	#include <stdio.h>  
	int/*medio*/main(void){  
	>>int i=42;  
	>>prontf("La respuesta es %d\n");

2. Preprocesar hello2.c, no compilar, y generar hello2.i. Analizar su contenido.

3. Escribir hello3.c, una nueva variante:
*	int printf(const char *s, ...);  
	int main(void){  
    >>int i=42;  
	>>prontf("La respuesta es %d\n");  
4. Investigar la semantica de la primera linea.
5. Preprocesar hello3.c, no compilar, y generar hello3.i. Buscar diferencias entre hello3.c y hello3.i.
6. Compilar el resultado y generar hello3.s, no ensamblar.
7. Corregir en el nuevo archivo hello4.c y empezar de nuevo, generar hello4.s, no ensamblar.
8. Investigar hello4.s
9. Ensamblar hello4.s en hello4.o, no vincular.
10. Vincular hello4.0 con la biblioteca estándar y generar el ejecutable.
11. Corregir en hello5.c y generar el ejecutable.
12. Ejecutar y analizar el resultado.
13. Corregir en hello6.c y empezar de nuevo.
14. Escribir hello7.c, una nueva variante:
*	int main(void){  
	>>int i=42;  
	>>printf("La respuesta es %d\n", i);  
	}
15. Explicar porque funciona.

### Resolucion
##### hello2.c
* **Comando:** gcc hello2.c -E -P -o hello2.i
* **Resultado:** Lo que obtengo utilizando este comando, es un archivo legible para el usuario en el cual se detalla todo el contenido del archivo cabecera stdio.h, mostrando todas sus definiciones y declaración de funciones(incluyendo la definción del tipo necesario para cada función). Tambien los comentarios son eliminados, no aparecen en el archivo.

##### hello3.c
* **Semantica de la primera linea:** Es una funcion del tipo 'int', por lo que debera devolver un valor entero, teniendo como primer argumento una cadena de caracteres que no sera modificada con un determinado formato. Ademas se le pueden agregar una ilimitada cantidad de otros argumentos los cuales printf() sabra que hacer con ellos.
* **Comando:** gcc hello3.c -E -P -o hello3.i 
* **Comparacion entre hello3.i y hello3.c:** No encontramos ninguna diferencia entre estos dos archivos(a excepción de la ausencia del comentario de presentacion del archivo, que no lo tendremos en cuenta porque el enunciado no pide que lo agregemos al código).
* **Comando:** gcc hello3.i -S
* **Resultado:** Lanza un Warning indicando que hay una funcion implicita para prontf(), pero con el nombre printf(), que si no deberiamos cambiarlo. Ademas lanza un error que indica que espera una declaracion final de la entrada, señalando la falta de una llave de cierra. El archivo hello3.s se crea, pero solamente contiene una linea de texto ".file	"hello3.i" ".

##### hello4.c
* **Comando:** gcc hello4.i -S
* **Resultado:** Crea un archivo que dentro tiene las instrucciones en codigo Assembler correspondiente a las acciones que debe hacer nuestro procesador para que el programa creado funcione. Analizamos este archivo y sus instrucciones mas importantes:
+ **Push rbp**: le hace un push al Base Pointer, agregandole un bloque de memoria. Tambien mueve el Stack Pointer(rsp).
+ **Mov rsp, rbp**: Mueve el valor contenido en el Stack Pointer hacia el Base Pointer.
+ **Sub 48, rsp**: Al valor contenido en el Stack Pointer, se le resta 48, agrandando bloques de memoria a la pila.
+ **Call __main**: Invoca a la funcion main() creada.
+ **Call prontf**: Invoca a la funcion prontf().
+ **Add 48, rsp**: Agrega 48 al valor del Stack Pointer y lo restablece. Es la operacion contraria a la echa en 'Sub 48, rsp'.
+ **Pop rbp**: Reestablece al valor inicial el Base Pointer. Es la operacion contraria a 'Push rbp'
+ **.LC0: .ascii "La respuesta es %d\12\0"**: Aqui es donde se guarda la cadena de texto que utilizara prontf().
* **Comando:** gcc hello4.s -C
* **Resultado:** Crea un archivo binario en codigo objeto listo para ser enlazado para producir lenguaje de máquina. El archivo es ilegible para el usuario.
* **Comando:**gcc hello4.o -o hello4
* **Resultado:** Lanza un error, debido a que no encuentra la funcion prontf() en la biblioteca estandar, por lo que no permite crear el ejecutable.

#####hello5.c
* **Comando:** gcc hello5.c -o hello6
* **Resultado:** Crea un archivo ejecutable por linea de comando, pero el resultado es distinto del esperado, en este caso es 1446736, esto se debe a que no esta aclarado el segundo argumento de la función printf(), correspondiente a '%d', por lo que se toma un valor arbitrario vamos a decir.

##### hello6.c
* **Comando:** gcc hello6.o -o hello6.exe
* **Resultado:** Crea un archivo ejecutable por linea de comando. Ahora si obtenemos el resultado esperado, que es el mensaje "El resultado es 42". Ya que agregamos la variable 'i' como segundo argumento de la función printf() que contiene el valor que deberia mostrar por pantalla.

##### hello7.c
* **¿Por qué funciona?:** El ejecutable del archivo hello7.c funciona y da un resultado correcto a pesar de no estar declarada la funcion printf(), debido a que el compilador asume que la funcion devuelve un entero y continua, ademas de estar la misma implícita(esto es avisado en un warning al pasar por el compilado).  El compilador espera que la declaracion de la funcion aparezca en la biblioteca estandar.   