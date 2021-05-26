# tkinter를 사용하기 위한 import
from tkinter import *
from tkinter import ttk
import pymysql


# MySQL와 연결
conn = pymysql.connect(host='localhost', user = 'root', password='k5j5y5@naver', db='login', charset='utf8')
cursor = conn.cursor()

# tkinter 객체 생성
window = Tk()

# 사용자 id와 password를 저장하는 변수 생성
id, pwd = StringVar(), StringVar()

# 사용자 id와 password를 비교하는 함수
def check_data():
    try:
        sql = "SELECT id,pwd FROM login WHERE id = %s"
        cursor.execute(sql,(id.get()))  # id.get()는 str
        # fetchone 함수로 cursor에 담겨진 하나의 데이터를 가져오기
        result = cursor.fetchone()      # result는 튜플형으로 만들어진다
        if  id.get() == result[0]  and  pwd.get() == result[1] : 
            login_success()

        else :
            print("You're Id or pwd is wrong")      
    
    # 회원이 아닐 시 출력 
    except Exception as e:
        print("You're not our member")
        """
        만약에 DB에 값이 없으면 id.get()은 result를 튜플형이 아닌, NoneType에러를 발생시킨다.
        그렇기 때문에 Exception처리로 이를 해결해줘야 한다.
        """

# 로그인 성공함수
def login_success():
    global login_success_screen
    
    # 로그인 창 객체생성 
    login_success_screen = Toplevel(window)
    login_success_screen.title("Success")
    login_success_screen.geometry("200x150")
    
    # 로그인 창 구성물 1 (보여질 text)
    Label(login_success_screen, text="").pack()
    Label(login_success_screen, text="Login Success").pack()
    Label(login_success_screen, text="").pack()
    Label(login_success_screen, text="").pack()
 
    # 로그인 창 구성물 2 (확인버튼)
    Button(login_success_screen, text="OK", command=delete_login_success, height=1, width=10).pack()

# 로그인 후 모든 창 닫는 함수
def delete_login_success():
    login_success_screen.destroy()
    window.destroy()
    """
    확인버튼을 누른 후 확인버튼창과 로그인창이 닫혀야 한다
    """



# Register 함수
def register():
    global username
    global password
    global username_entry
    global password_entry
    global register_screen

    register_screen = Toplevel(window) 
    register_screen.title("Register")
    register_screen.geometry("300x170")
 
    # 입력받은 걸 저장하는 변수 생성
    username = StringVar()
    password = StringVar()
    
    # Register창 구성물 1 (보여지는 text)
    username_lable = Label(register_screen, text="New ID")
    username_lable.pack()
 

    # Register창 구성물 2 (입력받는 bar)
    username_entry = Entry(register_screen, textvariable=username)
    username_entry.pack()
   
    # Register창 구성물 3 (보여지는 text)
    password_label = Label(register_screen, text="Password")
    password_label.pack()
    
    # Register창 구성물 4 (입력받는 bar)
    password_entry = Entry(register_screen, textvariable=password, show='*')
    password_entry.pack()
    
    # 공백으로 버튼과 입력란 간 틈 만들기
    Label(register_screen, text="").pack()
    
    # Register창 구성물 5 (Register 버튼)
    Button(register_screen, text="Register", width=10, height=1, command = register_user).pack()


# DB에 넣어주는 함수
def register_user():
 
    # 입력받은 값들을 변수에 담아주기
    username_info = username.get()
    password_info = password.get()

    # DB에 넣기
    sql = "INSERT INTO login(id,pwd) VALUES(%s, %s)"
    cursor.execute(sql,(username_info, password_info))
    conn.commit()
    
    # 처음에 입력받았던 username_enrty객체를 초기화
    username_entry.delete(0, END)
    password_entry.delete(0, END)
    Register_success()

# Register성공 함수
def Register_success():
    global Register_success_screen

    # 성공창 사이즈 및 title
    Register_success_screen = Toplevel(window)
    Register_success_screen.title("Success")
    Register_success_screen.geometry("200x150")
    
    # 성공창 구성물 1 (안내 txt)
    Label(Register_success_screen, text="").pack()
    Label(Register_success_screen, text="Register Success").pack()
    Label(Register_success_screen, text="").pack()
    Label(Register_success_screen, text="").pack()
 
    # 성공창 구성물 2 (확인버튼)
    Button(Register_success_screen, text="OK", command=delete_register_success, height=1, width=10).pack()

def delete_register_success():
    Register_success_screen.destroy()
    register_screen.destroy()
 



# id와 pwd, 그리고 확인 버튼의 UI를 만드는 부분
ttk.Label(window, text = "Username : ").grid(row = 0, column = 0, padx = 10, pady = 10)
ttk.Label(window, text = "Password : ").grid(row = 1, column = 0, padx = 10, pady = 10)
ttk.Entry(window, textvariable = id).grid(row = 0, column = 1, padx = 10, pady = 10)
ttk.Entry(window, textvariable = pwd, show='*').grid(row = 1, column = 1, padx = 10, pady = 10)
ttk.Button(window, text = "Login", command = check_data).grid(row = 3, column = 1, padx = 10, pady = 10)
ttk.Button(window, text = "Register", command = register).grid(row = 3, column = 0, padx = 10, pady = 10)


# GUI START
window.mainloop()

conn.close()
