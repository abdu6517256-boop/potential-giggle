import tkinter as tk
from tkinter import ttk

# -------------------
# الإعدادات الرئيسية للنافذة
# -------------------
root = tk.Tk()
root.title("التشرد - قضية اجتماعية وإنسانية")
root.geometry("800x600")
# خلفية بلون رمادي فاتح ودافئ
root.configure(bg="#f8f9fa") 

# -------------------
# دالة بناء قسم المحتوى
# -------------------
def add_section(parent, title, text):
    """تضيف قسماً جديداً (عنوان ونص) داخل الإطار الأب."""
    
    # الإطار الحاوية للقسم (Frame)
    frame = ttk.Frame(parent, padding="10")
    frame.pack(pady=15, padx=20, fill='x', anchor='n') # ملء العرض والمحاذاة للأعلى

    # عنوان القسم
    label_title = ttk.Label(
        frame, 
        text=title, 
        font=("Arial", 18, "bold"), 
        foreground="#005b9f" # لون أزرق غامق وواضح
    )
    label_title.pack(anchor='w') # محاذاة للجهة اليسرى (بداية النص العربي)

    # نص المحتوى
    label_text = ttk.Label(
        frame, 
        text=text, 
        wraplength=760, # يضمن التفاف النص وعدم خروجه عن العرض
        font=("Arial", 12),
        justify="right" # محاذاة النص لليمين (مناسب للغة العربية)
    )
    label_text.pack(anchor='w', pady=5) # محاذاة لليسار مع مسافة عمودية

# -------------------
# محتوى الأقسام المراد عرضها
# -------------------
sections = [
    ("ما هو التشرد؟",
     "التشرد هو حالة العيش بدون مأوى ثابت وآمن. إنه ليس مجرد غياب للمنزل، بل هو نقص حاد في الأمان، الكرامة، والوصول إلى الخدمات الأساسية كالصحة والتعليم."),
    ("أسباب التشرد الرئيسية",
     "تتنوع الأسباب بين العوامل الهيكلية (مثل نقص الإسكان الميسر والبطالة) والعوامل الفردية (كالأزمات الأسرية، الأمراض النفسية، والإدمان). وغالباً ما تتفاعل هذه الأسباب لتخلق حلقة مفرغة يصعب الخروج منها."),
    ("كيف نساهم في الحل؟",
     "يكمن الحل في المقاربات الشاملة التي تركز على توفير الإسكان أولاً (Housing First)، تليها خدمات الدعم النفسي والاجتماعي والتدريب المهني. يمكن للأفراد دعم هذه المبادرات عبر التطوع والتبرع وزيادة الوعي العام."),
]

# -------------------
# إنشاء منطقة التمرير (Scrollable Area)
# -------------------
# 1. إنشاء Canvas لحمل المحتوى وشريط التمرير
canvas = tk.Canvas(root, bg="#f8f9fa")
# 2. إنشاء شريط التمرير
scrollbar = ttk.Scrollbar(root, orient="vertical", command=canvas.yview)
# 3. إنشاء الإطار الذي سيحمل المحتوى الفعلي
scrollable_frame = ttk.Frame(canvas)

# ربط الإطار القابل للتمرير بـ Canvas ليعيد تحديد منطقة التمرير عند تغيير الحجم
scrollable_frame.bind(
    "<Configure>",
    lambda e: canvas.configure(
        scrollregion=canvas.bbox("all")
    )
)

# وضع الإطار داخل Canvas
canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")
# ربط Canvas بأوامر شريط التمرير
canvas.configure(yscrollcommand=scrollbar.set)

# ترتيب العناصر في النافذة الرئيسية
canvas.pack(side="left", fill="both", expand=True)
scrollbar.pack(side="right", fill="y")

# -------------------
# إضافة المحتوى إلى الإطار القابل للتمرير
# -------------------
for title, text in sections:
    add_section(scrollable_frame, title, text)

# -------------------
# بدء تشغيل التطبيق
# -------------------
root.mainloop()

