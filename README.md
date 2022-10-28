# Programacion-C-ejercicio-6
Ejercicio 6 de programacion en C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

#define CATEGORY1 1					/* Category 1 id */
#define CATEGORY2 2                 /* Category 2 id */
#define CATEGORY3 3                 /* Category 3 id */

#define MAX_NAME 15+1
#define MAX_CITY 15+1


typedef enum {STARTUPS=1, FREELANCERS, RURAL, SPECIALIZED, GENERALIST} tCoworkingType;

typedef struct {
    char name[MAX_NAME];
	char city[MAX_CITY];
    int category;
    tCoworkingType centerType;
	int numSpaces;
    float price; 
    float distanceFromCityCenter;
    bool hasMeetingRooms;
    bool hasAuditorium;
} tCoworkingCenter;

void readCenter(tCoworkingCenter *center);
void writeCenter(tCoworkingCenter center);
int bestCenter(tCoworkingCenter center1, tCoworkingCenter center2);
bool isAcceptableCenter (tCoworkingCenter center, float price, float distance);
int main(int argc, char **argv)
  {
    tCoworkingCenter center1; 
	tCoworkingCenter center2;
	float price;
	float distance;
    bool isAceptable1; 
	bool isAceptable2;
    int result;
    
/* Exercise 2.5 Procesamiento y salida de datos. Completar el algoritmo y mostrar por el canal
estándar de salida los resultados, de acuerdo con el siguiente flujo de ejecución:
● Leer, por el canal estándar de entrada, los datos de dos centros, el precio y
distancia máxima aceptables.
● Calcular si cada uno de los centros son aceptables usando la función
isAcceptableCenter (…). Si los dos centros son aceptables y están en la misma
ciudad, compararlos utilizando la función bestCenter(...) y mostrar los datos
mediante la acción writeCenter(...) de forma ordenada, es decir:
1. Si centro 1 es mejor o igual a centro 2, mostrar en primer lugar los datos del
centro 1.
2. En caso contrario, mostrar primeramente los datos del centro 2.
● En caso de que alguno de los centros no sea aceptable o que se encuentren en
distintas ciudades, mostrar mensaje indicando que los centros no pueden ser
comparables entre sí.
*/
    printf("ENTER DATA FOR CENTER 1\n");
    readCenter(&center1);

    printf("ENTER DATA FOR CENTER 2\n");
    readCenter(&center2);

    printf("ACCEPTABLE PRICE [EUR]? >>\n");
    scanf("%f", &price);
    printf("ACCEPTABLE DISTANCE FROM CITY CENTER [KM]? >>\n");
    scanf("%f", &distance);

    isAceptable1 = isAcceptableCenter (center1, price, distance);
    isAceptable2 = isAcceptableCenter (center2, price, distance);
		
	printf("RESULTS\n");
	
    if ((isAceptable1 && isAceptable2) && (strcmp(center1.city, center2.city)== 0))
    {
		result = bestCenter(center1, center2);
		if (result >= 0)
		{
			printf("CENTER 1 SUITS YOU BETTER, AND THE DATA ARE:\n");
			writeCenter(center1);
			printf("THE SECOND BEST CENTER IS CENTER 2 AND THE DATA ARE:\n");
			writeCenter(center2);
		}
		else
		{
			printf("CENTER 2 SUITS YOU BETTER, AND THE DATA ARE:\n");
			writeCenter(center2);
			printf("THE SECOND BEST CENTER IS CENTER 1 AND THE DATA ARE:\n");
			writeCenter(center1);
		}
	}
    else{
		printf("CENTERS CAN'T BE COMPARED\n");
	} 
    return 0;
  }


/* Exercise 2.1 Desarrollo de funciones/acciones. Desarrollar la acción readCenter(...) que reciba
como parámetro de salida una variable de nombre center, de tipo tCoworkingCenter sin
inicializar, y devuelva la misma variable con los campos informados con los datos leídos
por el canal estándar de entrada */

void readCenter(tCoworkingCenter *center)
{
	int intToBool;
	    
    printf("NAME? (A STRING) >>\n");
    scanf("%s",(*center).name);    
    printf("CITY? (A STRING)>>\n");
    scanf("%s",center->city);
    printf("CATEGORY? (1-CATEGORY1, 2-CATEGORY2, 3-CATEGORY3) >>\n");
    scanf("%d", &center->category);	
	printf("CENTER TYPE? (1-STARTUPS, 2-FREELANCERS, 3-RURAL, 4-SPECIALIZED, 5-GENERALIST) >>\n");
    scanf("%u", &center->centerType);
	printf("NUM. SPACES? (AN INTEGER) >>\n");
    scanf("%d", &center->numSpaces);
	printf("PRICE EUR? (A REAL) >>\n");
    scanf("%f", &center->price);    
	printf("DISTANCE TO CITY CENTER [KM]? (A REAL) >>\n");
    scanf("%f", &center->distanceFromCityCenter);	    
    printf("HAS MEETING ROOMS? (0-FALSE, 1-TRUE) >>\n");
    scanf("%d", &intToBool);
	center->hasMeetingRooms = (bool)intToBool;	
	printf("HAS AUDITORIUM? (0-FALSE, 1-TRUE) >>\n");
    scanf("%d", &intToBool);
	center->hasAuditorium = (bool)intToBool;
}

/* Exercise 2.2 Desarrollo de funciones/acciones. Desarrollar la función/acción writeCenter (...).
Esta recibe como parámetro de entrada una variable de nombre center, de tipo
tCoworkingCenter ya inicializado y muestra por el canal estándar de salida los campos del
centro. */

void writeCenter(tCoworkingCenter center)
{
	printf("CENTER'S DATA: \n");
	printf("NAME: %s\n", center.name);
	printf("CITY: %s\n", center.city);
	printf("CATEGORY (1-CATEGORY1, 2-CATEGORY2, 3-CATEGORY3): %u\n", center.category);
	printf("CENTER TYPE (1-STARTUPS, 2-FREELANCERS, 3-RURAL, 4-SPECIALIZED, 5-GENERALIST): %u\n", center.centerType);
	printf("NUM. SPACES: %d\n", center.numSpaces);
	printf("PRICE: %.2f\n", center.price);
	printf("DISTANCE TO CITY CENTER [KM]: %.2f\n", center.distanceFromCityCenter);	
	printf("HAS MEETING ROOMS (0-FALSE, 1-TRUE): %d\n", center.hasMeetingRooms);
	printf("HAS AUDITORIUM (0-FALSE, 1-TRUE): %d\n", center.hasAuditorium);	
}

/* Exercise 2.3 Desarrollo de funciones/acciones. Desarrollar la función/acción isAcceptableCenter
(…). Esta recibe como parámetros una variable de nombre center, de tipo
tCoworkingCenter inicializada, y dos variables price y distance de tipo real que
representan un precio y una distancia al centro de la ciudad aceptables. La función
retorna verdadero si el centro tiene sala de reuniones o auditorio, y tanto 
su precio como su distancia al centro de la ciudad se encuentran dentro de los valores aceptables
introducidos en los parámetros price y distance. */

bool isAcceptableCenter  (tCoworkingCenter center, float price, float distance)
{
	return ((center.hasMeetingRooms || center.hasAuditorium)  &&
			(center.distanceFromCityCenter <= distance) &&
			(center.price <= price));
}
  
/* Exercise 2.4 Desarrollo de funciones/acciones. Desarrollar la función/acción bestCenter(...).
Esta recibe como parámetros dos variables, center1 y center2, de tipo tCoworkingCenter
correspondientes a dos centros y los compara para ver cual es el mejor de ambos.
Para comparar los dos centros se seguirán los siguientes criterios en el orden indicado:
● El de mayor categoría es mejor. .
● El más cercano al centro de la ciudad es mejor.
● Tener una sala de reuniones es mejor.
En caso de que haya coincidencia en el primer criterio (los dos centros tienen la misma
categoría), se pasará al siguiente criterio, y así sucesivamente.*/

int bestCenter (tCoworkingCenter center1, tCoworkingCenter center2)
{
	int result = 0;   
		
	if (center1.category > center2.category)		
	{
		result = 1;
	} else {
		if (center1.category < center2.category)
		{
			result = -1;
		} else {							
			if (center1.distanceFromCityCenter < center2.distanceFromCityCenter) 
			{
				result = 1;
			} else {
				if (center1.distanceFromCityCenter > center2.distanceFromCityCenter) 
				{
					result = -1;
				} else  {
					if (center1.hasMeetingRooms && !center2.hasMeetingRooms)
					{
						result = 1;
					} else {
						if (!center1.hasMeetingRooms && center2.hasMeetingRooms) 
						{
							result = -1;							
						} 				 					
					}
				} 
			}			
		}
	}
	
    return(result);
  }

