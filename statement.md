from tkinter import*
from random import*
from tkinter import ttk, filedialog

from tkinter import colorchooser

color_fill="black"
image_number = 0

image1 = Image.new('RGB', (640, 400), 'white')


def about():
    top = Toplevel()
    top.title("About")
    top.minsize(width=200,height=100)
    label_about=Label(top,text="Это самое лучшее приложение")
    label_about.pack()

def circle():
    var_radio_shape.set(0)

def rectangle():
    var_radio_shape.set(1)

def line():
    var_radio_shape.set(2)

def vis_scale():
    if var_radio.get()==2:
        scale_2.place(x=200,y=125)
        lab_4.place(x=200,y=100)
    else:
        scale_2.place_forget()
        lab_4.place_forget()

def popup(event):
    global x, y
    x = event.x
    y = event.y
    menu_right.post(event.x_root, event.y_root)

def circle():
    var_radio_shape.set(0)

def save():
    global image_number
    filename=str(image_number) 
    image1.save(filename+".png")
    image_number += 1


def paint_mouse(event):
    draw = ImageDraw.Draw(image1)
    x = event.x
    y = event.y
    r=var_scale.get()
    dR=var_scale_2.get()
    
    if var_radio.get()==0:
        global color_fill
        color_fill="white"
        canvas.create_oval(x-r,y-r,x+r,y+r,fill=color_fill, outline="")
    elif ((var_radio.get()==1) and (var_radio_shape.get()==1)):
        canvas.create_rectangle(x+r,y+r,x-r,y-r,fill=color_fill,outline="")
        draw.rectangle((x+r,y+r,x-r,y-r),fill=color_fill)
    elif ((var_radio.get()==1) and (var_radio_shape.get()==2)):
        canvas.create_rectangle(x,y+r,x,y-r,fill=color_fill,outline="")
        draw.rectangle((x+r,y+r,x-r,y-r),fill=color_fill)
    elif ((var_radio.get()==2) and (var_radio_shape.get()==0)):
        for i in range(randint(3,dR)):
            canvas.create_oval(x+randint(-r,r),y+randint(-r,r),x-randint(-r,r),y-randint(-r,r),fill=color_fill, outline="black")
            draw.ellipse((x+randint(-r,r),y+randint(-r,r),x-randint(-r,r),y-randint(-r,r)),fill=color_fill,width=1,outline="black")
    elif ((var_radio.get()==2) and (var_radio_shape.get()==1)):

        for i in range(randint(3,dR)):
            canvas.create_rectangle(x+randint(-r,r),y+randint(-r,r),x-randint(-r,r),y-randint(-r,r),fill=color_fill, outline="black")
            draw.rectangle((x+randint(-r,r),y+randint(-r,r),x-randint(-r,r),y-randint(-r,r)),fill=color_fill,width=1,outline="black")
    else:
        canvas.create_oval(x-r,y-r,x+r,y+r,fill=color_fill, outline="")
        draw.ellipse((x-r,y-r,x+r,y+r),fill=color_fill,width=1,outline=color_fill)

def clean(event):
    canvas.delete('all')

def color_choose(event):
    global color_fill
    color=colorchooser.askcolor()
    # print(color)
    color_fill=color[1]


root=Tk()
root.geometry('640x580')
root.configure(background="light gray")
root.title("Paint")

mainmenu = Menu(root) 
root.configure(menu=mainmenu) 
 
filemenu = Menu(mainmenu, tearoff=0)
filemenu.add_command(label="Сохранить...",command=save)
filemenu.add_command(label="Выход",command=root.quit)
 
helpmenu = Menu(mainmenu, tearoff=0)
helpmenu.add_command(label="Помощь")
helpmenu.add_command(label="О программе",command=about)
 
mainmenu.add_cascade(label="Файл", menu=filemenu)
mainmenu.add_cascade(label="Справка", menu=helpmenu)

menu_right = Menu(tearoff=0)
menu_right.add_command(label="Круг",command=circle)
menu_right.add_command(label="Прямоугольник", command=rectangle)
menu_right.add_command(label="Линия",command=line)#

canvas=Canvas(root,width=640,height=400)
canvas.pack()
canvas.place(x=0,y=170)

canvas.bind("<B1-Motion>",paint_mouse)
canvas.bind("<Button-1>",paint_mouse)
canvas.bind("<Button-3>",popup)

lab_1 = Label(root,text="Инструменты")
lab_1.configure(background='light gray')
lab_1.pack()
lab_1.place(x=10,y=5)

var_radio=IntVar()
var_radio.set(1)
r_1=Radiobutton(text="Ластик",variable=var_radio,value=0)
r_1.configure(background='light gray')
r_1.pack()
r_1.place(x=10,y=30)

r_2=Radiobutton(text="Кисть",variable=var_radio,value=1,command=vis_scale)
r_2.configure(background='light gray')
r_2.pack()
r_2.place(x=10,y=50)


r_3=Radiobutton(text="Распылитель",variable=var_radio,value=2,command=vis_scale)
r_3.configure(background='light gray')
r_3.pack()
r_3.place(x=10,y=70)

lab_2 = Label(root,text="Форма")
lab_2.configure(background='light gray')
lab_2.pack()
lab_2.place(x=200,y=5)

var_radio_shape=IntVar()
var_radio_shape.set(0)
r_4=Radiobutton(text="Круг",variable=var_radio_shape,value=0)
r_4.configure(background='light gray')
r_4.pack()
r_4.place(x=200,y=30)

r_5=Radiobutton(text="Прямоугольник",variable=var_radio_shape,value=1)
r_5.configure(background='light gray')
r_5.pack()
r_5.place(x=200,y=50)

r_6=Radiobutton(text="Линия",variable=var_radio_shape,value=2)
r_6.configure(background='light gray')
r_6.pack()
r_6.place(x=200,y=70)


lab_3 = Label(root,text="Размер/Распыление")
lab_3.configure(background='light gray')
lab_3.pack()
lab_3.place(x=10,y=100)

var_scale=IntVar()
scale = Scale(root,from_=3, to=200, length=150, orient=HORIZONTAL, resolution=5, variable=var_scale)
scale.pack()
scale.place(x=10,y=125)

lab_4 = Label(root,text="Интенсивность")
lab_4.configure(background='light gray')
lab_4.place_forget()


var_scale_2=IntVar()
scale_2 = Scale(root,from_=3, to=20, length=150, orient=HORIZONTAL, resolution=3, variable=var_scale_2)
scale_2.place_forget()

lab_5 = Label(root,text="Палитра цвета")
lab_5.configure(background='light gray')
lab_5.pack()
lab_5.place(x=370,y=5)

btn_1 = Button(root, text="Очистить")
btn_1.configure(width=7,height=1)
btn_1.pack()
btn_1.place(x=520,y=30)
btn_1.bind("<Button-1>",clean)

btn_6 = Button(root, text="Палитра")
btn_6.configure(background="gray",width=7,height=1)
btn_6.pack()
btn_6.place(x=520,y=55)
btn_6.bind("<Button-1>",color_choose)

root.mainloop()
