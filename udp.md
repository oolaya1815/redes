RFC 768                                                        J. Postel
                                                                     ISI
                                                    28 de Agosto de 1980


                   PROTOCOLO DE DATAGRAMAS DE USUARIO
                        (User Datagram Protocol)

             (Traducción al castellano: Diciembre de 1999)
         (Por Domingo Sánchez Ruiz <domingo@quark.fis.ucm.es>)


Introducción

   Este Protocolo de Datagramas de Usuario (UDP: User Datagram Protocol)
   se define con la intención de hacer disponible un tipo de datagramas
   para la comunicación por intercambio de paquetes entre ordenadores en
   el entorno de un conjunto interconectado de redes de computadoras.
   Este protocolo asume que el Protocolo de Internet (IP: Internet
   Protocol) [1] se utiliza como protocolo subyacente.

   Este protocolo aporta un procedimiento para que los programas de
   aplicación puedan enviar mensajes a otros programas con un mínimo de
   mecanismo de protocolo.  El protocolo se orienta a transacciones, y
   tanto la entrega como la protección ante duplicados no se garantizan.
   Las aplicaciones que requieran de una entrega fiable y ordenada de
   secuencias de datos deberían utilizar el Protocolo de Control de
   Transmisión (TCP: Transmission Control Protocol). [2]


Formato

                     0      7 8     15 16    23 24    31
                    +--------+--------+--------+--------+
                    |    Puerto de    |    Puerto de    |
                    |      Origen     |     Destino     |
                    +--------+--------+--------+--------+
                    |                 |                 |
                    |    Longitud     | Suma de Control |
                    +--------+--------+--------+--------+
                    |
                    |          octetos de datos ...
                    +---------------- ...

               Formato de la Cabecera de un Datagrama de Usuario






J. Postel                                                       [Pág. 1]

RFC 768          Protocolo de Datagramas de Usuario       28 Agosto 1980


Campos

   El campo Puerto de Origen es opcional; cuando tiene sentido, indica
   el puerto del proceso emisor, y puede que se asuma que ése sea el
   puerto al cual la respuesta debería ser dirigida en ausencia de otra
   información. Si no se utiliza, se inserta un valor cero.

   El campo Puerto de Destino tiene significado dentro del contexto de
   una dirección de destino en un entorno internet particular.

   El campo Longitud representa la longitud en octetos de este datagrama
   de usuario, incluyendo la cabecera y los datos. (Esto implica que el
   valor mínimo del campo Longitud es ocho.)

   El campo Suma de Control (Checksum) es el complemento a uno de 16
   bits de la suma de los complementos a uno de las palabras de la
   combinación de una pseudo-cabecera construída con información de la
   cabecera IP, la cabecera UDP y los datos, y rellenada con octetos de
   valor cero en la parte final (si es necesario) hasta tener un
   múltiplo de dos octetos.

   La pseudo-cabecera que imaginariamente antecede a la cabecera UDP
   contiene la dirección de origen, la dirección de destino, el
   protocolo y la longitud UDP. Esta información proporciona protección
   frente a datagramas mal encaminados. Este procedimiento de
   comprobación es el mismo que el utilizado en TCP.

                     0      7 8     15 16    23 24    31
                    +--------+--------+--------+--------+
                    |        dirección de origen        |
                    +--------+--------+--------+--------+
                    |        dirección de destino       |
                    +--------+--------+--------+--------+
                    |  cero  |protocol|  longitud UDP   |
                    +--------+--------+--------+--------+

   Si la suma de control calculada es cero, se transmite como un campo
   de unos (el equivalente en la aritmética del complemento a uno). Un
   valor de la suma de control trasmitido como un campo de ceros
   significa que el el emisor no generó la suma de control (para
   depuración o para protocolos de más alto nivel a los que este campo
   les sea indiferente).


Interfaz de Usuario

   Un interfaz de usuario debería permitir:




J. Postel                                                       [Pág. 2]

RFC 768          Protocolo de Datagramas de Usuario       28 Agosto 1980


      la creación de nuevos puertos de recepción,

      operaciones de recepción en los puertos de recepción que devuelvan
      los octetos de datos y una indicación del puerto de origen y de la
      dirección de origen,

      y una operación que permita enviar un datagrama, especificando los
      datos y los puertos de origen y de destino y las direcciones a las
      que se debe enviar.



Interfaz IP

   El módulo UDP debe ser capaz de determinar las direcciones de origen
   y destino en un entorno internet así como el campo de protocolo de la
   cabecera del protocolo internet. Una posible interfaz UDP/IP
   devolvería el datagrama de internet completo, incluyendo toda la
   cabecera, en respuesta a una operación de recepción. Un interfaz de
   este tipo permitiría también al módulo UDP pasar un datagrama de
   internet completo con cabecera al módulo IP para ser enviado. IP
   verificaría ciertos campos por consistencia y calcularía la suma de
   control de la cabecera del protocolo internet.


Aplicación del Protocolo

   Los usos principales de este protocolo son el Servidor de Nombres de
   Internet [3] y la Transferencia Trivial de Ficheros (Trivial File
   Transfer) [4].


Número del protocolo

   Este es el protocolo 17 (21 en octal) cuando se utilice en el
   Protocolo de Internet (IP). Se indican otros números de protocolo en
   [5].


Referencias


[1]  Postel,   J.,   "Internet  Protocol,"  RFC 760,  USC/Information
     Sciences Institute, Enero de 1980.  (Nota del T. Hay traducción al
     español por P.J. Ponce de León: "Protocolo Internet", Mayo 1999.)

[2]  Postel,   J.,   "Transmission   Control   Protocol,"   RFC 761,
     USC/Information Sciences Institute, Enero de 1980.



J. Postel                                                       [Pág. 3]

RFC 768          Protocolo de Datagramas de Usuario       28 Agosto 1980


[3]  Postel,   J.,  "Internet  Name Server,"  USC/Information Sciences
     Institute, IEN 116, Agosto de 1979.

[4]  Sollins,  K.,  "The TFTP Protocol,"  Massachusetts  Institute of
     Technology, IEN 133, Enero de 1980.

[5]  Postel,   J.,   "Assigned   Numbers,"  USC/Information  Sciences
     Institute, RFC 762, Enero de 1980.


Nota del traductor

     Este documento y las traducciones al español mencionadas en las
     referencias pueden encontrarse en:
     http://lucas.hispalinux.es/htmls/estandares.html

     El proyecto de traducción de RFC al español tiene su
     web de desarrollo en:
     http://www.arrakis.es/~pjleon/rfc-es

J. Postel                                                       [Pág. 4]
