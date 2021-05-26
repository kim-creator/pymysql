# tkinter를 사용하기 위한 import
from tkinter import *
from tkinter import ttk

import pymysql

conn = pymysql.connect(host='localhost', user = 'root', password='k5j5y5@naver', db='login', charset='utf8')
cursor = conn.cursor()

# tkinter 객체 생성
window = Tk()

# 사용자 id와 password를 저장하는 변수 생성
id, pw = StringVar(), StringVar()

# 사용자 id와 password를 비교하는 함수
def check_data():
    try:
        sql = "SELECT id,pw FROM client WHERE id = %s"
        cursor.execute(sql,(id.get()))
        result = cursor.fetchone()
        
        if id.get() == result[0] and pw.get() == result[1]:
            print("login_success")

        else:
            print("You're Id or Pw is wrong")


        #회원 아닐시 출력

    except Exception as e:
            print("You're not our member")

# id와 password, 그리고 확인 버튼의 UI를 만드는 부분
ttk.Label(window, text = "Username : ").grid(row = 0, column = 0, padx = 10, pady = 10)
ttk.Label(window, text = "Password : ").grid(row = 1, column = 0, padx = 10, pady = 10)
ttk.Entry(window, textvariable = id).grid(row = 0, column = 1, padx = 10, pady = 10)
ttk.Entry(window, textvariable = pw).grid(row = 1, column = 1, padx = 10, pady = 10)
ttk.Button(window, text = "Login", command = check_data).grid(row = 2, column = 1, padx = 10, pady = 10)

window.mainloop()
