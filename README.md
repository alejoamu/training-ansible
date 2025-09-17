# training-ansible
## Paso a paso
### Conexión y configuración 
1. Lo primero que tenemos que hacer es verificar el estado de la maquina virtual en la cual vamos a ejecutar el programa. Verificar que este corriendo y tomar datos como:
  - IP Publica
  - usuario
  - Contraseña
2. Posteriormente agregaremos esos datos al archivo de hosts para poder hacer la conexion a la vm
![Imagen de WhatsApp 2025-09-17 a las 09 31 22_ad4d674f](https://github.com/user-attachments/assets/9133a8e5-c4fd-45ee-b7ca-5f4dec618fbe)

3. Ahora accedemos al wsl para poder hacer uso de ansible, usando el comando "wsl -d ubuntu" (ubuntu ya que es el sistema que fue instalado en la maquina para la resolución de este laboratorio). Y verificamos que ansible este correctamente instalado
![Imagen de WhatsApp 2025-09-17 a las 09 31 38_686e19fb](https://github.com/user-attachments/assets/98738482-1861-45dc-9e7f-5e208c697719)
4. Ahora verificamos la conexion hacia la maquina virtual, haciendo uso del comando "ansible all -m ping"
![Imagen de WhatsApp 2025-09-17 a las 09 31 58_0a1719b2](https://github.com/user-attachments/assets/6cf7e0e1-c5fa-41f8-92c8-94148a8e27b2)
En este caso el problema se presentó porque al ejecutar ansible all -m ping no se había definido un inventario de hosts, por lo que Ansible solo reconocía el localhost implícito y este no coincidía con el grupo all, dejando el comando sin hosts a los cuales conectarse.

Para solucionarlo, copiamos el proyecto a una ruta dentro del home (~), evitando la advertencia de directorio con permisos inseguros y asegurando que Ansible pudiera reconocer correctamente la configuración (ansible.cfg), el inventario (inventory/) y el resto de la estructura del laboratorio.

![Imagen de WhatsApp 2025-09-17 a las 09 32 15_1da69113](https://github.com/user-attachments/assets/94bd0db2-0bbf-457b-ba8b-f0570939c0ea)

5. Ahora volvemos a probar el ping

![Imagen de WhatsApp 2025-09-17 a las 09 32 36_f4b213eb](https://github.com/user-attachments/assets/109a03be-af07-4640-b8dc-214f4b2e6a33)

El error ocurrió porque Ansible intentó conectarse por SSH usando usuario y contraseña, pero para este método necesita el programa sshpass, el cual no estaba instalado en el sistema, generando el fallo al ejecutar ansible all -m ping. 

Para solucionarlo, instalamos la utilidad sshpass (sudo apt-get install sshpass), para permitir a Ansible manejar autenticación con contraseñas y establecer correctamente la conexión hacia el host remoto.
![Imagen de WhatsApp 2025-09-17 a las 09 32 57_b34dc1d1](https://github.com/user-attachments/assets/8b0c2c08-17ca-4b68-a43a-4f6cd025d14f)

6. Volvemos a probar el ping y vemos como en esta ocasión funciona correctamente
![Imagen de WhatsApp 2025-09-17 a las 09 33 09_4420732d](https://github.com/user-attachments/assets/7ccc0ef4-f77f-46c6-8b8c-3fd0bd45a2d0)
7. Ahora podemos hacer el proceso de descarga e instalacion de los playbooks necesarios para ejecutar el programa
![Imagen de WhatsApp 2025-09-17 a las 09 33 21_1bcf6214](https://github.com/user-attachments/assets/7ba31dd3-0222-4c95-b08b-f510edcdc0aa)
8. Ejecutamos el comando de ejecucion de los playbook
![Imagen de WhatsApp 2025-09-17 a las 09 33 32_e17211aa](https://github.com/user-attachments/assets/dffa0d2a-00b1-4351-836b-e91cf10c2590)


### Ejecución

1. Ahora podemos ingresar a la ip publica de la vm para visualizar el despliegue
<img width="1862" height="977" alt="image" src="https://github.com/user-attachments/assets/82a1390c-9077-4a4e-b255-77840336e6e8" />
Este error ocurre debido a que la maquina virtual solamente tiene el puerto 22 abierto para la conexión, pero necesita que los puertos de visualización web sean configurados para permitir la conexion del mismo.

Por lo que entramos a la maquina virtual en el portal de azure y seguimos los siguienes pasos:
- Ingresar Rededs
- Configuración de red
- Crear ACL del puerto
- Configura:
Puerto:

Para configurar el puerto adecuado, tenemos que verificar la configuracion del contenedor y ver por que puerto esta saliendo

<img width="960" height="538" alt="image" src="https://github.com/user-attachments/assets/486c7627-09c2-4cd8-9076-7117bded2d42" />

En este caso tenemos que configurar el 8787 para poder visualizar el sistema, entonces:

Puerto: 8787
Protocolo: TCP
Acción: Permitir
Prioridad: 1000
Nombre: Mario-8787

![Imagen de WhatsApp 2025-09-17 a las 09 30 56_d1c903fe](https://github.com/user-attachments/assets/6540f3c7-16ff-4368-ab0c-c3a8cf6f662d)

2. Intetamos el ingreso de nuevo
![Imagen de WhatsApp 2025-09-17 a las 09 30 33_15cd41af](https://github.com/user-attachments/assets/5c0a158c-a06b-41c3-a36d-c49508e0f817)

Y vemos como en efecto se conecta a la aplicacion desplegada en la vm.
