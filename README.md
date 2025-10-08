

## 🚀 نحوه‌ی اجرا

۱. فایل کد را با نام `navasan.py` در پوشه‌ی دلخواه ذخیره کنید.
۲. سپس در همان مسیر، در CMD یا Terminal اجرا کنید:

```bash
python navasan.py
```

۳. یک پنجره با پس‌زمینه‌ی مشکی و متن قرمز ظاهر می‌شود که به‌صورت روان قیمت‌ها را نشان می‌دهد.

---

## 🧩 کد اصلی برنامه

```python
import tkinter as tk
import requests
import threading
import time

API_URL = "http://api.navasan.tech/latest/?api_key=freeQM3JQ9i7f76ZVizWRAzFGq9HkkGO"

# ------------------------------
# تابع دریافت داده از API
# ------------------------------
def fetch_prices():
    try:
        response = requests.get(API_URL, timeout=10)
        data = response.json()

        # نمونه داده‌هایی که ممکن است موجود باشند
        usd_buy = data.get("usd_buy", {}).get("value", "?")
        usd_sell = data.get("usd_sell", {}).get("value", "?")
        gold18 = data.get("geram18", {}).get("value", "?")
        mesghal = data.get("mesghal", {}).get("value", "?")

        text = (
            f"دلار خرید: {usd_buy} | دلار فروش: {usd_sell} | "
            f"طلا 18 عیار: {gold18} | مثقال طلا: {mesghal}"
        )
        return text

    except Exception as e:
        return f"❌ خطا در دریافت اطلاعات: {e}"

# ------------------------------
# بروزرسانی قیمت‌ها
# ------------------------------
def update_prices():
    global current_text
    while True:
        new_text = fetch_prices()
        if new_text != current_text:
            current_text = new_text
        time.sleep(60)

# ------------------------------
# حرکت متن مثل تابلو روان
# ------------------------------
def move_text():
    global x_pos
    canvas.delete("all")
    canvas.create_text(
        x_pos, 40,
        text=current_text,
        font=("Arial", 22, "bold"),
        fill="red"
    )
    x_pos -= 3  # سرعت حرکت
    if x_pos < -len(current_text) * 14:
        x_pos = 900
    root.after(50, move_text)

# ------------------------------
# رابط گرافیکی
# ------------------------------
root = tk.Tk()
root.title("تابلو روان قیمت دلار و طلا - Navasan")
root.configure(bg="black")
root.geometry("900x100")
root.resizable(False, False)

canvas = tk.Canvas(root, bg="black", height=80, width=900, highlightthickness=0)
canvas.pack(pady=10)

current_text = "در حال دریافت اطلاعات از سرور..."
x_pos = 900

# شروع حرکت متن
move_text()

# شروع رشته جداگانه برای بروزرسانی قیمت‌ها
threading.Thread(target=update_prices, daemon=True).start()

root.mainloop()
```

---

## 🔗 منبع داده‌ها

قیمت‌ها از API سایت [Navasan.tech](https://api.navasan.tech) خوانده می‌شوند:

```
http://api.navasan.tech/latest/?api_key=freeQM3JQ9i7f76ZVizWRAzFGq9HkkGO
```

### 📦 نمونه‌ی داده‌ی JSON:

```json
{
  "usd_buy": {
    "value": "11270",
    "change": -25,
    "timestamp": 1568212950,
    "date": "1398-06-20 19:12:30"
  },
  "usd_sell": {
    "value": "11290",
    "change": -30,
    "timestamp": 1568212950,
    "date": "1398-06-20 19:12:30"
  }
}
```

---

## 🖼️ نمای برنامه

```
🟥🟥🟥  دلار خرید: 11270 | دلار فروش: 11290 | طلا 18 عیار: 4150000 | مثقال طلا: 17985000  🟥🟥🟥
```

(در نسخه‌ی واقعی متن قرمز به‌صورت روان روی پس‌زمینه‌ی مشکی حرکت می‌کند.)

---

## 👨‍💻 توسعه‌دهنده

پروژه توسط **[MILAD AHMADI]** ساخته شده است.
اگر از این پروژه خوشت اومد، ⭐️ بده و فورک کن 😊

---

## 📜 مجوز

این پروژه تحت مجوز **MIT License** منتشر شده است و برای استفاده‌ی آزاد، آموزشی و شخصی مجاز می‌باشد.

```

