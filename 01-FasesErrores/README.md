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
	
	#include <stdio.h>
	int/*medio*/main(void){
		int i=42;
 		prontf("La respuesta es %d\n");

2. Preprocesar hello2.c, no compilar, y generar hello2.i. Analizar su contenido.

3. Escribir hello3.c, una nueva variante:
	int printf(const char *s, ...);
	int main(void){
		int i=42;
		prontf("La respuesta es %d\n");
4. Investigar la semantica de la primera linea.
5. Preprocesar hello3.c, no compilar, y generar hello3.i. Buscar diferencias entre hello3.c y hello3.i.
6. Compilar el resultado y generar hello3.s, no ensamblar.
7. Corregir en el nuevo archivo hello4.c y empezar de nuebo, generar hello4.s, no ensamblar.
8. Investigar hello4.s
9. Ensamblar hello4.s en hello4.o, no vincular.
10. Vincular hello4.0 con la biblioteca estándar y generar el ejecutable.
11. Corregir en hello5.c y generar el ejecutable.
12. Ejecutar y analizar el resultado.
13. Corregir en hello6.c y empezar de nuevo.
14. Escribir hello7.c, una nueva variante:
	int main(void){
		int i=42;
 		printf("La respuesta es %d\n", i);
	}
15. Explicar porque funciona.


### Resolucion

##### hello2.c

* **Comando:** gcc hello2.c -E -P -o hello2.i
* **Resultado:** Lo que obtengo utilizando este comando, es un archivo legible para el usuario en el cual se detalla todo el contenido del archivo cabecera stdio.h, mostrando todas sus definiciones y declaración de funciones(incluyendo la definción del tipo necesario para cada función). Tambien los comentarios son eliminados, no aparecen en el archivo.

##### hello3.c
* **Semantica de la primera linea:** Esta linea representa lo que seria la funcion 'printf()' dentro de stdio.h. Haciendo el analisis semantico obtenemos que: 
	+ Es una función de tipo **int**, lo que devolvera un valor entero. Este sera el número de caracteres impresos en pantalla, o un número negativo en caso de haber algun error.
	+ Su primer argumento, **const char *s **, se trata de una cadena de caracteres(char *) que no sera modificada por la funcion (const), con lo que puede ser una constante de cadena o una variable que contenga una cadena, pero siempre debe acabar en el caracter nulo. En cuanto a la **s**, se refiere al array que contiene la cadena de caracteres, si la funcion se realiza correctamentem devuelve 0, de lo contrario algo distinto de 0. 
	+ Su segundo argumento, **...**, signica que podemos poner una ilimitada cantidad de otros argumentos los cuales printf() sabra que hacer con ellos.
* **Comando:** gcc hello3.c -E -P -o hello3.i 
* **Comparacion entre hello3.i y hello3.c:** No encontramos ninguna diferencia entre estos dos archivos(a excepción de la ausencia del comentario de presentacion del archivo, que no lo tendremos en cuenta porque el enunciado no pide que lo agregemos al código).
* **Comando:** gcc hello3.i -S
* **Resultado:** Lanza un Warning indicando que hay una funcion implicita para prontf(), pero con el nombre printf(), que si no deberiamos cambiarlo. Ademas lanza un error que indica que espera una declaracion final de la entrada. El archivo hello3.s se crea, pero solamente contiene una linea de texto ".file	"hello3.i"".