# Hamscod-
햄스코드입니다. 단점:심각한건데 더연구해봐야 할것 같습니다 
import tkinter as tk
from tkinter import messagebox
from datetime import datetime

# 테마 색상 설정 (카카오톡 느낌)
BG_MAIN = "#FFFFFF"      # 채팅방 배경 (스카이블루)
BG_HEADER = "#0081FF"    # 상단 바 배경
BG_INPUT = "#FFFFFF"     # 입력창 영역 배경
BTN_YELLOW = "#F5F5F5"   # 전송 버튼 (카카오 옐로우)
BTN_GRAY = "#E0E0E0"     # 일반 버튼
TEXT_DARK = "#191919"    # 기본 글자색
TIME_COLOR = "#556677"   # 시간 표시 글자색

def send_message(event=None):
    msg = entry.get().strip()
    if msg:
        now = datetime.now().strftime("%p %I:%M").replace("AM", "오전").replace("PM", "오후")
        chat_box.config(state=tk.NORMAL)
        
        # 시간과 메시지를 각각 다른 스타일(태그)로 우측 정렬 출력
        chat_box.insert(tk.END, f"{now}  ", "time")
        chat_box.insert(tk.END, f" {msg} \n\n", "my_msg")
        
        chat_box.config(state=tk.DISABLED)
        chat_box.see(tk.END)
        entry.delete(0, tk.END)

def export_chat():
    content = chat_box.get("1.0", tk.END).strip()
    if not content:
        messagebox.showwarning("알림", "저장할 채팅 내용이 없습니다.")
        return
    
    filename = f"chat_{datetime.now().strftime('%Y%m%d_%H%M%S')}.txt"
    with open(filename, "w", encoding="utf-8") as f:
        f.write(content)
    messagebox.showinfo("완료", f"채팅 내역이 저장되었습니다:\n{filename}")

root = tk.Tk()
root.title("Pydroid 3 GUI")
root.configure(bg=BG_MAIN)

# 상단 타이틀 바
header_frame = tk.Frame(root, bg=BG_HEADER, pady=10)
header_frame.pack(fill=tk.X)

label = tk.Label(header_frame, text="Hamscod", font=("Helvetica", 15, "bold"), bg=BG_HEADER, fg=TEXT_DARK)
label.pack()

# 채팅 기록 출력 영역 (테두리 제거 및 배경색 통일)
chat_box = tk.Text(
    root, height=16, width=38, state=tk.DISABLED,
    font=("Helvetica", 12), bg=BG_MAIN, fg=TEXT_DARK,
    bd=0, highlightthickness=0, padx=12, pady=10, wrap=tk.WORD
)
chat_box.pack(expand=True, fill=tk.BOTH)

# 말풍선 효과 및 정렬을 위한 텍스트 태그 설정
chat_box.tag_configure("my_msg", justify="right", background=BTN_YELLOW, foreground=TEXT_DARK)
chat_box.tag_configure("time", justify="right", foreground=TIME_COLOR, font=("Helvetica", 9))

# 하단 입력창 영역
input_frame = tk.Frame(root, bg=BG_INPUT, pady=6, padx=8)
input_frame.pack(fill=tk.X)

entry = tk.Entry(input_frame, font=("Helvetica", 12), bg="#F5F5F5", fg=TEXT_DARK, bd=0, highlightthickness=1, highlightbackground="#F5F5F5")
entry.pack(side=tk.LEFT, expand=True, fill=tk.X, ipady=6, padx=(0, 6))
entry.bind("<Return>", send_message)

send_btn = tk.Button(
    input_frame, text="전송", command=send_message,
    font=("Helvetica", 11, "bold"), bg=BTN_YELLOW, fg=TEXT_DARK,
    activebackground="#F5F5F5", bd=0, padx=12, pady=5
)
send_btn.pack(side=tk.RIGHT)

# 최하단 기능 버튼 영역 (내보내기 / 나가기)
bottom_frame = tk.Frame(root, bg=BG_MAIN, pady=8)
bottom_frame.pack(fill=tk.X)

btn_container = tk.Frame(bottom_frame, bg=BG_MAIN)
btn_container.pack()

export_btn = tk.Button(
    btn_container, text="채팅 내보내기", command=export_chat,
    font=("Helvetica", 10), bg="#F5F5F5", fg=TEXT_DARK, bd=0, padx=10, pady=4
)
export_btn.pack(side=tk.LEFT, padx=5)

btn = tk.Button(
    btn_container, text="나가기", command=root.destroy,
    font=("Helvetica", 10), bg=BTN_GRAY, fg=TEXT_DARK, bd=0, padx=10, pady=4
)
btn.pack(side=tk.LEFT, padx=5)

root.mainloop()
