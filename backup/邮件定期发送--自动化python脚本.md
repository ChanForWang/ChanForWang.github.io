因为工作原因，每周周四团队的秘书就会要求我们更新最新一个礼拜的weekly schedule (excel)，透过邮件发给她，因为除了修改excel的内容，其他可谓都是重复性的动作(写发邮件，上传附件等)，事虽小，但很浪费时间，且自己也总是忘记；

因此今天上班突发奇想：能不能创建一个自动化的流程来自动执行这一切，自己只需更新个人shedule就好！？

然后说干就干，仔细思考了下整个流程逻辑：
1.每周四下午2点30分，自动提醒我要更新weekly schedule-->提示框
2.随后自动帮我打开对应Excel文档，让我更新-->open excel file
3.当我完成更新后，提示我是否发送邮件-->提示框
4.Yes：则检查Outlook是否开启，未开启则帮我先启动再以附件形式发送，并提醒已发送完成！
5.No：则没事，并提醒已取消发送！

代码如下：

`
import schedule
import time
import os
import tkinter as tk
from tkinter import messagebox
import win32com.client as win32
import psutil

def show_reminder(msg):
    root = tk.Tk()
    root.withdraw() # 隐藏窗口
    messagebox.showinfo("Notification",msg)
    root.destroy()

def open_excel():

    os.startfile(r"C:\Users\James Chan\OneDrive - Pico Group\Desktop\HKGMC-Projects\z.Weekly Schedule-James\Weekly Schedule-James.xlsx")

def send_email():
    # 检查Outlook是否已经打开
    outlook_running = False
    for proc in psutil.process_iter(['name']):
        if proc.info['name'] == 'OLK.EXE': #可透过task manager来查看名称！
            outlook_running = True
            break

    # 如果Outlook没有运行，启动Outlook(new)
    if not outlook_running:
        os.startfile(r"C:\Program Files\WindowsApps\Microsoft.OutlookForWindows_1.2024.403.300_x64__8wekyb3d8bbwe\olk.exe")
        print("Outlook is not running. Starting Outlook...")
        # 等待Outlook启动，这里假设Outlook启动需要大约10秒
        time.sleep(10)

    try:
        outlook = win32.Dispatch('outlook.application')
        mail = outlook.CreateItem(0)
        mail.Subject = 'Weekly Schedule Testing'
        mail.Body = 'Dear Josephine,\n\nAttached please find my latest weekly schedule, thanks.\n\nBest Regards\nJames Chan'
        mail.To = 'josephine.tsang@pico.com'
        mail.CC = 'james.chan@pico.com'
        mail.Attachments.Add(r"C:\Users\James Chan\OneDrive - Pico Group\Desktop\HKGMC-Projects\z.Weekly Schedule-James\Weekly Schedule-James.xlsx")
        mail.Send()
    except Exception as e:
        print(f"Failed to send email: {e}")

def show_2nd_reminder_and_send_email():
    root = tk.Tk()
    root.withdraw()
    response = messagebox.askyesno("提醒", "是否发送邮件给Jo？")
    root.destroy()
    if response:
        send_email()
        show_reminder("已发送完成！")
    else:
        show_reminder("已取消发送！")

def job():
    # 这里放置你的任务代码
    show_reminder("记得发送本周weekly schedule")
    open_excel()
    time.sleep(5)  # 等待5秒
    show_2nd_reminder_and_send_email()

schedule.every().thursday.at("14:30").do(job)
while True:
    schedule.run_pending()
    time.sleep(21600) #每隔6小时检查一次：6*60*60s；不用担心当时间到了1430，而程序还在sleep中不执行，因为run_pending解决了这个事！

`

代码部分是完成了，但由于这只是个python script，无法做到24小时全天候监听事件发生，除非一直打开并运行这个python.py；
但显然这样的做法太傻！

所以经过一番研究，本想采用windows的service来后台执行，但一直报错，所以摒弃这个方式，并采取第二种曲线救国方式：
**Task Scheduler**;

透过设置运行条件为：登录系统就自动执行程序来自启动运行，可以省却自己去启动脚本的繁琐过程！

但因为python.py是脚本，虽然task scheduler也可以运行，但不够简洁和无法满足我需要它在后台运行的需求，因此需要将脚本文件透过以下命令转换为无窗口运行的exe程序：

`pyinstaller --onefile --noconsole AutoEmail.py`

这样就会将py文件转换成exe文件，最后task scheduler设置完毕后，每次自动登录系统，便会在后台自启动运行！

![image](https://github.com/ChanForWang/ChanForWang.github.io/assets/52204491/ef915d93-e8a7-4f9c-bf50-946285a1beb5)
![image](https://github.com/ChanForWang/ChanForWang.github.io/assets/52204491/cc094559-2111-4d77-886d-d1bb1c264f0a)

