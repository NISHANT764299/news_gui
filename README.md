# news_gui

import webbrowser
import requests
from tkinter import *
from urllib.request import urlopen
from PIL import ImageTk,Image 
import io



class newsapp():
    def __init__(self):

       self.data = requests.get('https://newsapi.org/v2/everything?q=bitcoin&apiKey=3555e2f8cb7f45db82866247c8b99fe9').json()
       self.load_gui()

       self.load_news_item(0)
    def load_gui(self):
        self.root = Tk()
        self.root.geometry('350x600')
        self.root.resizable(0,0)
        self.root.configure(background= 'black')

    def clear(self):
        for i in self.root.pack_slaves():
            i.destroy()


    def load_news_item(self,index):

        self.clear()
        try:
            img_url = self.data['articles'][index]['urlToImage']
            raw_data = urlopen(img_url).read()
            img = Image.open(io.BytesIO(raw_data)).resize((350,250))
            photo = ImageTk.PhotoImage(img)

        except:
            img_url = "https://preview.redd.it/help-this-image-couldnt-be-loaded-error-and-i-need-that-v0-71omzkrcy1la1.png?width=271&format=png&auto=webp&s=fe951e015ceb11f9990a1328ad7332d539c2ad8b"
            raw_data = urlopen(img_url).read()
            img = Image.open(io.BytesIO(raw_data)).resize((350,250))
            photo = ImageTk.PhotoImage(img)



        label = Label(self.root,image=photo)
        label.pack()

        heading_label = Label(self.root,text =self.data['articles'][index]['title'],bg ='black',fg='white',
                            wraplength=350,justify='center')
        heading_label.pack(pady=(10,20))
        heading_label.config(font = ('verdana',15))

        details_label = Label(self.root,text =self.data['articles'][index]['description'],bg ='black',fg='white',
                            wraplength=350,justify='center')
        details_label.pack(pady=(2,20))
        details_label.config(font = ('verdana',12))

        frame = Frame(self.root,bg = 'black')
        frame.pack(expand=True,fill=BOTH)

        if index !=0:
           prev = Button(frame, text='Previous',width=16,height=3,command=lambda :self.load_news_item(
            index-1))
           prev.pack(side=LEFT)

        read = Button(frame, text='Read here',width=16,height=3)
        read.pack(side=LEFT)
        
        if index != len(self.data['articles'])-1:
           next = Button(frame, text='Next',width=16,height=3,command=lambda :self.load_news_item(
            index+1))
           next.pack(side=LEFT)

    #def open_link(self,url):
     #      webbrowser.open('https://www.wired.com/story/bitcoin-bros-go-wild-for-donald-trump/')


        self.root.mainloop()




obj = newsapp()
