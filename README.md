# Job_Simulator
This program uses the First Come First Save and Priority scheduling 
from tkinter import *
import time
import random

class simulator:
    def __init__(self):
        self.tags = []
        self.prioritynotOk = True
        self.jobname=[]
        self.priority = []
        self.arrtime = []
        self.turntime = []
        self.gui = Tk()
        self.gui.configure(background = "black")
        self.tr = 0
        self.gui.title("JOB SIMULATOR")
        self.gui.geometry("900x600")
        self.m = Menu(self.gui)
        self.gui.config(menu=self.m)
        filemenu = Menu(self.m)
        self.m.add_cascade(label='OPTIONS', menu=filemenu)
        filemenu.add_command(label='Reset', command = self.clear)
        filemenu.add_separator()
        filemenu.add_command(label='Quit', command=sys.exit)
        filemenu.add_separator()

        topFrame = Frame(self.gui)
        topFrame.pack()
        midFrame = Frame(self.gui)
        midFrame.pack()
        bottomFrame = Frame(self.gui)
        bottomFrame.pack()
        bottFrame = Frame(self.gui)
        bottFrame.pack()


        l01 = Label(topFrame, text="Enter job name please:", fg="red")
        #l01.configure(background="purple")
        l01.pack(side=LEFT)
        self.a1 = StringVar()
        self.a = Entry(topFrame,textvariable = self.a1)
        self.a.pack(side=LEFT)


        l02 = Label(midFrame, text="\nEnter Priority   :\n(0. lp, 1. hp)", fg="red")
        #l02.configure(background="purple")
        l02.pack(side=LEFT)
        self.b1 = IntVar()
        self.b = Entry(midFrame)
        self.b.pack(side=LEFT)


        l03 = Label(bottomFrame, text="\nEnter Arrival Time  :", fg="red")
        #l03.configure(background="purple")
        l03.pack(side=LEFT)
        self.c1= IntVar()
        self.c = Entry(bottomFrame)
        self.c.pack(side=LEFT)


        l04 = Label(bottFrame, text="\nEnter turn around time (TAT seconds):", fg="red")
        #l04.configure(background="purple")
        l04.pack(side=LEFT)
        self.d1 = IntVar()
        self.d = Entry(bottFrame)
        self.d.pack(side=LEFT)


        self.l05 = Label(self.gui, text="\n")
        #self.l05.configure(background="purple")
        self.l05.pack()
        self.myanimationcanvas = Canvas(self.gui, width=900, height=250, bg='white')
        self.myanimationcanvas.pack()
        self.l06 = Label(self.gui, text="\n")
        #self.l06.configure(background="purple")
        self.l06.pack()
        self.dowork = Button(self.gui, bg="blue", width=20, height=5, text="ADD JOB ",
                             command=self.sortjobs)
        self.dowork.config(bg='black', fg='red', bd=10)
        self.dowork.pack()
        self.gui.mainloop()

    def sortjobs(self):
            self.jobname.append(self.a.get())
            self.priority.append(self.b.get())
            self.arrtime.append(self.c.get())
            self.turntime.append(self.d.get())
            self.createthejobrectangles()

    def createthejobrectangles(self):
        colors=['blue','green','yellow','brown','red','white']
        process = []
        color = random.choice(colors)
        text = "Job number: " + str(len(self.jobname)) + "\nJob name: " + str(
              self.jobname[len(self.jobname) - 1]) + "\nPriority: " + \
                 str(self.priority[len(self.priority) - 1]) + "\nArrival time: " + str(
              self.arrtime[len(self.arrtime) - 1]) + "\nTurnaround time:" + str(
              self.turntime[len(self.turntime) - 1]) + ""
        rect = Button(self.myanimationcanvas, text=text, bg=color, width=25, height=6)
        rect.config(bd=8)
        rect.pack()
        thejob = self.myanimationcanvas.create_window(25, 50*2, window=rect)
        process.append(thejob)
        time.clock()
        jobswillswap = True
        passnum = len(self.priority) - 1
        if (len(self.priority) == 1):
            for x in range(0, 830):
                self.myanimationcanvas.move(ALL, 1, 0)
                self.myanimationcanvas.update()
                time.sleep(.04)
                stop = time.clock()
            if stop:
                self.myanimationcanvas.delete(thejob)
                self.priority.pop(len(self.priority) - 1)
        elif (len(self.priority) > 1):
            while passnum > 0 and jobswillswap:
                jobswillswap = False
                for b in range(len(self.priority), 0, -1):
                    for i in range(b):
                        if self.priority[i + 1] > self.priority[i]:
                            for x in range(0, 830):
                                self.myanimationcanvas.move(thejob, 1, 0)
                                self.myanimationcanvas.update_idletasks()
                                self.myanimationcanvas.update()
                                time.sleep(.02)
                                stop = time.clock()
                            if stop:
                                self.myanimationcanvas.delete(thejob)
                                self.priority.pop(len(self.priority) - 1)
                                print("Done!")
                        else:
                            for x in range(0, 830):
                                self.myanimationcanvas.move(ALL, 1, 0)
                                self.myanimationcanvas.update()
                                time.sleep(.02)
                                stop = time.clock()
                            if stop:
                                self.myanimationcanvas.delete(thejob)
                                self.priority.pop(len(self.priority) - 1)
                            self.myanimationcanvas.delete(ALL)




    def clear(self):
        self.jobname.clear()
        self.priority.clear()
        self.turntime.clear()
        self.arrtime.clear()
        self.myanimationcanvas.delete(ALL)

simulator()
