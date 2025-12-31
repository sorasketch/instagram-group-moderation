# instagram-group-moderation
instagram group moderation botu küfür ve hakaret uyarısı
from bad_words import LIGHT, HEAVY
from logger import log_yaz

uyari_sayaci = {}

def normalize(text):
    return text.lower().replace(" ", "").replace(".", "").replace("*", "")

def kufur_tespit(mesaj):
    temiz = normalize(mesaj)
    for w in HEAVY:
        if w in temiz:
            return "HEAVY"
    for w in LIGHT:
        if w in temiz:
            return "LIGHT"
    return None

def mesaj_geldi(kullanici, mesaj, grup_id):
    seviye = kufur_tespit(mesaj)
    if not seviye:
        return

    uyari_sayaci[kullanici] = uyari_sayaci.get(kullanici, 0) + 1

    if seviye == "LIGHT":
        uyar = f"@{kullanici} bu grupta küfür yasak"
    else:
        uyar = f"@{kullanici} ağır küfür tespit edildi admin bilgilendirildi"

    print(uyar)
    log_yaz(kullanici, mesaj, seviye)
		LIGHT = [
    "salak",
    "aptal"
]

HEAVY = [
    "anne",
    "baba",
    "dini",
    "ağırküfür"
]
from datetime import datetime

def log_yaz(kullanici, mesaj, seviye):
    with open("log.txt", "a", encoding="utf8") as f:
        f.write(f"{datetime.now()} | {kullanici} | {seviye} | {mesaj}\n")
