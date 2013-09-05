LCD Alfanumerico (HD44780)
---------------------------
-----------

Pieza de código para controlar un lcd alfanumérico externo con el controlador Hitachi **hd44780** o compatible. este código solo se encarga de incializar el lcd, así como mandar comandos y/o datos al lcd, los detalles internos de comunicación se manejan a través del código que maneja la interfaz 6800 en 4 u 8 bits.

Se necesita implementar funciones para escribir en la memoria de gráficos del lcd "CGRam" 

Este codigo es dependiente de los archivos **types.h**, **6800/_6800.h**, y **middleware_profile.h** 


####Ejemplo de uso
Envió de cadenas de datos a diferentes posiciones de un lcd de 4x20
```

#include <p18cxxx.h>
#include "vectors.h"
#include "types.h"
#include "hd44780/hd44780.h"

#pragma code
void main(void)
{
    ANCON0 = 0XFF;  /*Desactivamos las salidas analogicas*/
    ANCON1 = 0XFF;  /*Desactivamos las salidas analogicas*/

    HD44780_Init(); /*Se iniciliaza el lcd*/

    HD44780_WriteString("Hola mundo1");
    HD44780_SetCursor(1,1);
    HD44780_WriteString("Hola mundo1");
    HD44780_SetCursor(2,2);
    HD44780_WriteString("Hola mundo1");
    HD44780_SetCursor(3,3);
    HD44780_WriteString("Hola mundo1");
    while (1)
    {

    }
}
```

####Configuración
Para seleccionar el tamaño del bus de datos se define en el archivo middleware_profile.h el siguiente define 
```
#define _6800_BUSLENGHT         4 /*tamaño del bus en la interfaz 6800*/
```

Para seleccionar el numero de columnas y filas en el lcd se escriben las siguientes lineas en el archivo modules_profile.h
```
#define HD44780_ROWS			2 /*numero de filas (valor por default)*/  
#define HD44780_COLUMNS			16/*numero de columnas (valor por default)*/
```

####API 
```
	/*-- Functions --*/
    /**---------------------------------------------------------------------------------------------    
      \brief      Inicializa el controlador hd44780, sin cursor, dos lineas y fuente 5x8
      \param	  None
      \return     None
      \warning	  Esta función usa de retardos y ciclara al uC al menos 70ms
    ----------------------------------------------------------------------------------------------*/
    void HD44780_Init(void);
    
    /**---------------------------------------------------------------------------------------------    
      \brief      pregunta al controlador si esta ocupado o puede recibir otro dato/comando
      \param	  None
      \return     '1' si esta ocupado
      \warning	  None
    ----------------------------------------------------------------------------------------------*/
    _BOOL HD44780_bBusyFlag(void);
    
    /**---------------------------------------------------------------------------------------------    
      \brief      Mueve el curso a columna y fila especifica
      \param	  u8Row.- fila (de 0 a HD44780_ROWS-1)
      \param	  u8Column.- columna (de 0 a HD44780_COLUMNS-1)
      \return     None
      \warning	  Trava al uC por espacio de 40us
    ----------------------------------------------------------------------------------------------*/
    void HD44780_SetCursor(const _U08 u8Row, const _U08 u8Col);

    /**---------------------------------------------------------------------------------------------    
      \brief      Manda un simple dato a la memoria interna de datos
      \param	  u8Data.- dato a enviar
      \return     None
      \warning	  trava al uC por 40us
    ----------------------------------------------------------------------------------------------*/
    void HD44780_WriteData(const _U08 u8Data);

    /**---------------------------------------------------------------------------------------------    
      \brief      Escribe una cadena terminada en cero, almacenada en flash,
      \param	  strString.- puntero a cadena terminada en cero
      \return     None
      \warning	  Trava al uC 40us por cada carácter enviado
    ----------------------------------------------------------------------------------------------*/
    void HD44780_WriteString(const rom _S08 *strString);
```

####Ejemplos
Descomprime estos ejemplos en el mismo directorio donde tengas tu banco de código.

- [Ejemplo 1: Envió de mensajes en diferentes posiciones del lcd][1]
- [Ejemplo 2: Uso de la funciones printf con el lcd][2]

  [1]: http://www.hotboards.org/images/codigo/8bits/examples/hd447801.zip
  [2]: http://www.hotboards.org/images/codigo/8bits/examples/hd447802.zip