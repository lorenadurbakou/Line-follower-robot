# Line-follower-robot
Εργασία Πανεπιστημίου στο εργαστήριο Ενσωματωμένα Συστήματα . για το τμήμα πληροφορικής του τμήματος ΔΠΘ
...
from machine import Pin, PWM
import time
''ΚΙΝΗΤΗΡΕΣ''
in1 = Pin(0, Pin.OUT)
in2 = Pin(1, Pin.OUT)
in3 = Pin(2, Pin.OUT)
in4 = Pin(3, Pin.OUT)

ena = PWM(Pin(4))
enb = PWM(Pin(5))
ena.freq(1000)
enb.freq(1000)

''ΑΙΣΘΗΤΉΡΕΣ''
sensor_left = Pin(10, Pin.IN)
sensor_center = Pin(11, Pin.IN)
sensor_right = Pin(12, Pin.IN)

''ΤΑΧΥΤΗΤΕΣ''
BASE_SPEED = 32000  
TURN_SPEED = 42000  

last_direction = 0

'' ΣΥΝΑΡΤΗΣΕΙΣ ΚΙΝΗΣΗΣ''
def move_forward():
    in1.high()
    in2.low()
    in3.high()
    in4.low()
    ena.duty_u16(BASE_SPEED)
    enb.duty_u16(BASE_SPEED)

def turn_left():
    in1.low()
    in2.low()
    in3.high()
    in4.low()
    ena.duty_u16(0)
    enb.duty_u16(TURN_SPEED)

def turn_right():
    in1.high()
    in2.low()
    in3.low()
    in4.low()
    ena.duty_u16(TURN_SPEED)
    enb.duty_u16(0)

def stop_all():
    in1.low()
    in2.low()
    in3.low()
    in4.low()
    ena.duty_u16(0)
    enb.duty_u16(0)


    while True:
        left = sensor_left.value()
        center = sensor_center.value()
        right = sensor_right.value()
        
        if center == 0 and left == 1 and right == 1:
            move_forward()
            last_direction = 0
            
        elif left == 0 and right == 1:
            turn_left()
            last_direction = 1
            
        elif right == 0 and left == 1:
            turn_right()
            last_direction = 2
            
        elif center == 0 and left == 0 and right == 0:
            move_forward()
            
        else:
            if last_direction == 1:
                turn_left()
            elif last_direction == 2:
                turn_right()
            else:
                move_forward()
            
        time.sleep(0.005)

        

except KeyboardInterrupt:
    stop_all()
