## ডাটাবেইজ কুয়েরি, জ্যাঙ্গো ওআরএম এর কুয়েরিসেট এপিআই!

মডেল তৈরি (এবং ডাটাবেইজ তৈরি) করার পর আমাদের কাজ হল সেখানে ডাটা ইনসার্ট করা, সেখানের ডাটা রিড, আপডেট বা ডিলেট করা। জ্যাঙ্গো ওআরএম আমাদেরকে এই কাজগুল করার জন্য সুন্দর একটা এপিআই দেয়, কুয়েরিসেট! 
কোডে চলে যাই, প্রথমেই কমান্ড প্রম্পট বা টার্মিনালে জ্যাঙ্গো শেল ওপেন করুনঃ

    python manage.py shell
    
পাইথন ইন্টারএকটিভ শেল চালু হবে, সেখানে models.py তে তৈরি করা আমাদের Message মডেল (ক্লাস)  ইম্পোর্ট করুনঃ

    from myapp.models import Message

আমরা Message মডেল দিয়ে ডাটাবেইজ তৈরি করে ফেলেছি, এখন এই মডেল ব্যবহার করেই সেই ডাটাবেইজ এর অপারেশন গুলো করব। আমরা জানি যে এই মডেলটির প্যারেন্ট ক্লাস হল জ্যাঙ্গোর models.Model ক্লাস। এই মডেল ক্লাসটিই ডাটাবেইজ অপারেশনগুলো করার জন্য আমাদেরকে objects নামে একটা ম্যানেজার (এপিআই) দেয়। আমরা সেটা আমাদের Message ক্লাসে ব্যবহার করব।

প্রথমেই দেখা যাক ডাটাবেইজের সব ডাটা রিড করা যায় কিভাবে, লিখুনঃ
 
    Message.objects.all()

Message ক্লাসের Obojects ম্যানেজার এর all() মেথডকে কল করলাম, এটা আমাদের ডাটাবেইজ কুয়েরি করে সকল ডাটা কুয়েরিসেট হিসেবে রিটার্ন করবে! উপরের কোড লিখে এন্টার করলে আপনি পরের লাইনে এরকম কিছু দেখতে পাবেনঃ
    
    <QuerySet []>

কুয়েরিসেট অবজেক্ট, একটা খালি লিস্ট! খালি থাকার কারণ হল আমরা এখনো ডাটাবেইজে কোন ডাটা রাখিনি। আসুন আগে কিছু ডাটা (মেসেজ আরকি) রাখি। ডাটা ক্রিয়েট করতে এই কমান্ডঃ

    Message.objects.create(text=”Hi there! How are You!”)
    Message.objects.create(text=”Have a nice day boy!”)
    Message.objects.create(text=”Are you Crazy !”)

আমরা তিনটি মেসেজ বা ডাটা তৈরি করলাম, `objects` ম্যানেজারের `create` ফাংশন কল করে ডাটা ক্রিয়েট করেছি, ফাংশনের ভেতর কিওয়ার্ড আর্গুমেন্ট হিসেবে ডাটাবেইজের কলামের নাম এবং ফিল্ড ভ্যালু (সেই কলামের ডাটা যেটা হবে) দিয়েছি। এখন আমাদের ডাটাবেইজের `Message` টেবিলের `text` কলামে তিনটা ফিল্ড/রো যুক্ত হয়েছে, তিনটা মেসেজ!

এবার যদি আমরা `Message.objects.all()` কল করি তাহলে এরকম দেখবঃ

    <QuerySet [<Message: Hi there! how are you!>, <Message: Have a nice day boy!>, <Message: Are you Crazy!>]>

কুয়েরিসেট লিস্ট, ভিতরে তিনটি আইটেম, আমাদের তিনটি মেসেজ!

`Objects` এর `all()` ফাংশনটি সকল আইটেম কুয়েরি করে বের করে। আমরা যদি নির্দিষ্ট কোন একটা আইটেম বের করতে চাই তাহলে `get()` ফাংশন ব্যবহার করতে পারিঃ

    Message.objects.get(id=1)

আমাদের মডেলে যদিও আমরা শুধু `text` এট্রিবিউট বা কলাম রেখেছিলাম, কিন্তু জ্যাঙ্গো আমাদের জন্য ‘প্রাইমারি কি’ হিসেবে অতিরিক্ত আরেকটা এট্রিবিউট যোগ করে দিয়েছে `id` নামে। এটা করার কারন হল আমরা কোন ফিল্ডকে প্রাইমারি কি হিসেবে সেট করিনি, কিন্তু জ্যাঙ্গো ওআরএম প্রতিটি মডেলের মধ্যেই একটা ‘প্রাইমারি কি’ আশা করে।

> যদিও অতিরিক্ত কলামটির নাম id  তবে সেটাকে pk নামেও ডাকা যাবে। pk = Primary Key !

get() ফাংশনটিতে আমরা id=1 দিয়ে বলেছি যে সে যেন আমাদেরকে ঐ ফিল্ডটা রিটার্ন করে যার id এর ভ্যালু হল ১।

`Message.objects.get(id=1)` লিখলে রিটার্ন আসবেঃ
    <Message: Hi there! how are you!>

এবার কোন কুয়েরিসেট/লিস্ট আসেনি, বরং একটাই মাত্র অবজেক্ট! আমরা অবজেক্টটি থেকে আমাদের প্রয়োজন অনুযায়ী ফিল্ড ভ্যালু বের করে নিতে পারিঃ

    >>> mytext = Message.objects.get(id=1)
    <Message: Hi there! how are you!>
    >>> mytext.text
    “Hi there! How are you!”
    >>> mytext.id
    1
    >>> mytext.pk
    1

আমাদের মডেলে যদি আরো ফিল্ড থাকত তাহলে আমরা এভাবে সেগুলোর ভ্যালুও বের করতে পারতাম।

`Obojects` ম্যানেজারের আরেকটা বহুল ব্যবহৃত মেথড হল `filter()`, অনেক সময়ই আমাদেরকে নির্দিষ্ট কোন কন্ডিশনের উপর ভিত্তি করে ডাটাবেইজের ডাটা রিড করতে হয়। ফিল্টার মেথড আমাদেরকে সেই কাজটি করতে সাহায্য করেঃ

    >>> mytext = Message.objects.filter(id=1)