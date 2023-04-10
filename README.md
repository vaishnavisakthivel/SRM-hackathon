# SRM-hackathon
import threading
from time import sleep


class Road:
    def __init__(self, vehicle, lanes):
        self.vehicle = vehicle
        self.ambulance = False
        self.lanes = lanes
        self.status = False


def add_vehicle():
    print("-----------------------------------------------------------------------")
    num = 1
    for road in roads:
        t = int(input("Enter no of vehicles in Road " + str(num) + " : "))
        num = num + 1
        road.vehicle = road.vehicle + t


def signal_timer(road):
    timer = 30
    wait_time = int(road.vehicle / road.lanes)

    if wait_time < timer:
        timer = wait_time
    tot = road.lanes
    road.status = True
    while timer > 0:
        sleep(1)
        timer -= 1
        road.vehicle = road.vehicle - tot
    road.status = False


def get_status():
    print("-----------------------------------------------------------------------")
    print("Printing current road status : ")
    print()
    num = 1
    for road in roads:
        print("Road " + str(num) + " status : ")
        num = num + 1
        if road.status:
            color = "green"
        else:
            color = "Red"
        print("  Signal : " + color)
        print("  Available Vehicles : " + str(road.vehicle))


def signal_controller():
    while True:
        for road in roads:
            signal_timer(road)


def get_input():
    print("-----------------------------------------------------------------------")
    print("1. Add more Vehicles")
    print("2. Road status")
    print("3. Exit")
    val = input("Select any option : ")
    if val == "1":
        add_vehicle()
        get_input()
    elif val == "2":
        get_status()
        get_input()
    elif val == "3":
        print("Exiting...")
    else:
        print("Enter valid Input")
        get_input()


n = int(input("Enter number of Roads : "))
roads = []
for i in range(n):
    r = int(input("Enter no of vehicles in Road " + str(i + 1) + " : "))
    l = int(input("Enter number of lanes in Road " + str(i + 1) + " : "))
    t = Road(r, l)
    roads.append(t)

t1 = threading.Thread(target=signal_controller)
t1.daemon = True
t1.start()
get_input()
