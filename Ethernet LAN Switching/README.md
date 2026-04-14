## En este laboratorio analicé como funciona ARP en una red LAN y como los switches aprenden por si mismos las direcciones MAC de los dispositivos en una red LAN.

## Objetivos 
- Comprender el proceso ARP (ARP request/ ARP reply).
- Observar como los switches aprenden direcciones MAC.
- Revisar la tabla de direcciones MAC de los switches.
- Eliminar entradas dinámicas de las tablas de direcciones MAC.

## Procedimiento
1. Generar tráfico (PC1 a PC3)
   Se generó tráfico haciendo ping de PC1 a PC3, iniciando el proceso ARP para resolver la dirección IP de PC3 a su dirección MAC.

2. Revisión de tabla de direcciones MAC
   Se ejecutó el siguiente comando en SW1 y en SW2:

   show mac address-table

   Esto con el fin de observar como las direcciones MAC se aprenden dinámicamente.

3. Generar tráfico (PC2 a PC4)
   Se generó tráfico haciendo ping ahora de PC2 a PC4, iniciando un nuevo proceso ARP para resolver la dirección IP de PC4 a su dirección MAC.

4. Revisión de tabla de direcciones MAC
   Se ejecutó nuevamente el comando:

   show mac address-table

   Con el fin de observar las nuevas direcciones MAC aprendidas.

5. Eliminar direcciones MAC
   Se ejecutó el siguiente comando en SW1 Y SW2:

   clear mac address-table dynamic

   Para eliminar las direcciones MAC aprendidas dinámicamente.

## Aprendizaje
   - Los switches aprenden las direcciones MAC apartir del tráfico.
   - Generar tráfico como ping ayuda a llenar las tablas de direcciones MAC de los switches.
   - ARP es fundamental para las comunicaciones iniciales entre dispositivos.
