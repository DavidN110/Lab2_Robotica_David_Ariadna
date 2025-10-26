from robodk.robolink import *    # API para comunicarte con RoboDK
from robodk.robomath import *    # Funciones matemáticas
import math

#------------------------------------------------
# 1) Conexión a RoboDK e inicialización
#------------------------------------------------
RDK = Robolink()
Target_Home2 = RDK.Item("Target Home2",ITEM_TYPE_TARGET)

# Elegir un robot (si hay varios, aparece un popup)
robot = RDK.ItemUserPick("Selecciona un robot", ITEM_TYPE_ROBOT)
if not robot.Valid():
    raise Exception("No se ha seleccionado un robot válido.")

# Conectar al robot físico
#if not robot.Connect():
   # raise Exception("No se pudo conectar al robot. Verifica que esté en modo remoto y que la configuración sea correcta.")

# Confirmar conexión
#if not robot.ConnectedState():
    #raise Exception("El robot no está conectado correctamente. Revisa la conexión.")

#print("Robot conectado correctamente.")

#------------------------------------------------
# 2) Cargar el Frame (ya existente) donde quieres dibujar
#    Ajusta el nombre si tu Frame se llama diferente
#------------------------------------------------
frame_name = "Frame_from_Target1"
frame = RDK.Item(frame_name, ITEM_TYPE_FRAME)
if not frame.Valid():
    raise Exception(f'No se encontró el Frame "{frame_name}" en la estación.')

# Asignamos este frame al robot
robot.setPoseFrame(frame)
# Usamos la herramienta activa
robot.setPoseTool(robot.PoseTool())

# Ajustes de velocidad y blending
robot.setSpeed(300)   # mm/s - Ajusta según necesites
robot.setRounding(5)  # blending (radio de curvatura)

#------------------------------------------------
# 3) Parámetros de la figura (rosa polar)
#------------------------------------------------
num_points = 300       # Cuántos puntos muestreamos (mayor = más suave)
A = 150               # Amplitud (300 mm = radio máximo)
k = 4                 # Parámetro de la rosa (pétalos). Si es impar, habrá k pétalos; si es par, 2k
z_surface = 0          # Z=0 en el plano del frame
z_safe = 50            # Altura segura para aproximarse y salir

#------------------------------------------------
# 4) Movimiento al centro en altura segura
#------------------------------------------------
# El centro de la rosa (r=0) corresponde a x=0, y=0
robot.MoveJ(Target_Home2)

robot.MoveJ(transl(0, 0, z_surface - z_safe))
# Bajamos a la "superficie" (Z=0)
#robot.MoveL(transl(0, 0, z_surface))

#------------------------------------------------
# 5) Dibujar la rosa polar
#    r = A * sin(k*theta)
#    x = r*cos(theta), y = r*sin(theta)
#------------------------------------------------
# Recorremos theta de 0 a 2*pi (una vuelta completa)
full_turn = 2*math.pi

for i in range(num_points+1):
    # Fracción entre 0 y 1
    t = i / num_points
    # Ángulo actual
    theta = full_turn * t

    # Calculamos r
    r = A * math.cos(k * theta)

    # Convertimos a coordenadas Cartesianas X, Y
    x = r * math.cos(theta)
    y = r * math.sin(theta)

    # Movemos linealmente (MoveL) en el plano del Frame
    robot.MoveL(transl(x, y, z_surface))

# Al terminar, subimos de nuevo para no chocar
robot.MoveL(transl(x, y, z_surface - z_safe))

print(f"¡Figura (rosa polar) completada en el frame '{frame_name}'!")


#------------------------------------------------
#7) Nombres de los integrantes
# ARIADNA (arriba) y DAVID (abajo)
#------------------------------------------------
x_offset_ariadna = 250   # separación vertical en X
x_offset_david   = 150
#------------------------------------------------

#-------------------ARIADNA------------------------
#----A----
robot.MoveL(transl(x_offset_ariadna + 30, -315, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 30, -315, z_surface))
robot.MoveL(transl(x_offset_ariadna + 130, -315, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, -300, z_surface))
robot.MoveL(transl(x_offset_ariadna + 130, -285, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, -285, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, -315, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, -285, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -285, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -285, z_surface - z_safe))

#----R----
robot.MoveL(transl(x_offset_ariadna + 150, -255, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 150, -255, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30,  -255, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -255, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 150, -255, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 150, -255, z_surface))
robot.MoveL(transl(x_offset_ariadna + 120, -205, z_surface))
robot.MoveL(transl(x_offset_ariadna + 90, -255, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -205, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -205, z_surface- z_safe))


#----I----
robot.MoveL(transl(x_offset_ariadna + 150, -195, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 150, -195, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, -165, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, -180, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30,  -180, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30,  -195, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30,  -165, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30,  -165, z_surface - z_safe))

#----A----
robot.MoveL(transl(x_offset_ariadna + 30, -135, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 30, -135, z_surface))
robot.MoveL(transl(x_offset_ariadna + 130, -135, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, -120, z_surface))
robot.MoveL(transl(x_offset_ariadna + 130, -105, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, -105, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, -135, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, -105, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -105, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -105, z_surface - z_safe))

#----D----
robot.MoveL(transl(x_offset_ariadna + 30, -75, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 30, -75, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, -75, z_surface))
robot.MoveL(transl(x_offset_ariadna + 90, -25, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -75, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, -75, z_surface - z_safe))

#----N----
robot.MoveL(transl(x_offset_ariadna + 30, -15, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 30, -15, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, -15, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30,  15, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, 15, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, 15, z_surface - z_safe))

#----A----
robot.MoveL(transl(x_offset_ariadna + 30, 45, z_surface - z_safe))
robot.MoveL(transl(x_offset_ariadna + 30, 45, z_surface))
robot.MoveL(transl(x_offset_ariadna + 130, 45, z_surface))
robot.MoveL(transl(x_offset_ariadna + 150, 60, z_surface))
robot.MoveL(transl(x_offset_ariadna + 130, 75, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, 75, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, 45, z_surface))
robot.MoveL(transl(x_offset_ariadna + 80, 75, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, 75, z_surface))
robot.MoveL(transl(x_offset_ariadna + 30, 75, z_surface - z_safe))


#-------------------DAVID------------------------
#----D----
robot.MoveL(transl(x_offset_david + 150, 105, z_surface - z_safe))
robot.MoveL(transl(x_offset_david + 150, 105, z_surface))
robot.MoveL(transl(x_offset_david + 30,  105, z_surface))
robot.MoveL(transl(x_offset_david + 90,  155, z_surface))
robot.MoveL(transl(x_offset_david + 150, 105, z_surface))
robot.MoveL(transl(x_offset_david + 150, 105, z_surface - z_safe))

#----A----
robot.MoveL(transl(x_offset_david + 30, 165, z_surface - z_safe))
robot.MoveL(transl(x_offset_david + 30, 165, z_surface))
robot.MoveL(transl(x_offset_david + 130, 165, z_surface))
robot.MoveL(transl(x_offset_david + 150, 180, z_surface))
robot.MoveL(transl(x_offset_david + 130, 195, z_surface))
robot.MoveL(transl(x_offset_david + 80, 195, z_surface))
robot.MoveL(transl(x_offset_david + 80, 165, z_surface))
robot.MoveL(transl(x_offset_david + 80, 195, z_surface))
robot.MoveL(transl(x_offset_david + 30, 195, z_surface))
robot.MoveL(transl(x_offset_david + 30, 195, z_surface - z_safe))

#----V----
robot.MoveL(transl(x_offset_david + 150, 225, z_surface - z_safe))
robot.MoveL(transl(x_offset_david + 150, 225, z_surface))
robot.MoveL(transl(x_offset_david + 30, 240, z_surface))
robot.MoveL(transl(x_offset_david + 150, 255, z_surface))
robot.MoveL(transl(x_offset_david + 30, 225, z_surface - z_safe))

#----I----
robot.MoveL(transl(x_offset_david + 150, 255, z_surface - z_safe))
robot.MoveL(transl(x_offset_david + 150, 255, z_surface))
robot.MoveL(transl(x_offset_david + 150, 285, z_surface))
robot.MoveL(transl(x_offset_david + 150, 270, z_surface))
robot.MoveL(transl(x_offset_david + 30, 270, z_surface))
robot.MoveL(transl(x_offset_david + 30, 285, z_surface))
robot.MoveL(transl(x_offset_david + 30, 255, z_surface))
robot.MoveL(transl(x_offset_david + 30, 255, z_surface - z_safe))

#----D----
robot.MoveL(transl(x_offset_david + 30, 315, z_surface - z_safe))
robot.MoveL(transl(x_offset_david + 30, 315, z_surface))
robot.MoveL(transl(x_offset_david + 150,  315, z_surface))
robot.MoveL(transl(x_offset_david + 90,  355, z_surface))
robot.MoveL(transl(x_offset_david + 30, 315, z_surface))
robot.MoveL(transl(x_offset_david + 30, 315, z_surface - z_safe))

# Posición segura final
robot.MoveL(transl(0, 0, z_surface - 2*z_safe))
robot.MoveJ(Target_Home2)
print(f"¡Figura completada con los nombres ARIADNA (X={x_offset_ariadna}) y DAVID (X={x_offset_david}) sobre el frame '{frame_name}'!")
