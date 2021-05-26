import pymysql
from tkinter import *
conn=pymysql.connect(host='localhost',user='root',password='k5j5y5@naver',db='login',charset='utf8')
cursor=conn.cursor()
window=Tk()
id,pwd=StringVar(),StringVar()
def check_data(): 
    try:
        sql="SELECT id,pwd FROM loginWHERE id=%s"
        cursor.execute(sql,(id.get()))
        result=cursor.fetchone()
        if id.get()==result[0] and pwd.get()==result[1]:
            login_success()

        else:
                print("Your ID or pwd is wrong")
    except Exception as e:
                    print("You're not our member")
