# Algoritma-Analizi-Odev_2.KısaSınav_2.ÖDev

import cv2
import glob


# Template Matching fonksiyonu :
def template_matching(template, image):
    method = cv2.TM_CCOEFF_NORMED
    result = cv2.matchTemplate(image, template, method)
    min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(result)
    return max_val

# Template Matching uygulanacak görsel bir şekilde
image_path = "resim/21.png"
image = cv2.imread(image_path)



# Template dosyalarının bulunduğu dizin :
templates_dir = "resim/*.png"

# Template dosyalarını okuma ve benzerlik oranlarını hesaplatan fonksiyon :
templates = glob.glob(templates_dir)
similarities = []
for template_path in templates:
    template = cv2.imread(template_path)

    # Template'yi 20x20 piksel boyutunda ölçeklendiren fonksiyonlar :
    width = 20
    height = 20
    resized_template = cv2.resize(template, (width, height))
    similarity = template_matching(resized_template, image)
    similarities.append((template_path, similarity))


# Benzerlik oranlarına göre sıralama :
similarities.sort(key=lambda x: x[1], reverse=True)



# Benzerlik oranında virgülden sonraki kısmı 5 basamağını sayı olarak alır.

def get_decimal_places(number):
    decimal_part = str(number).split('.')[1]  # Ondalık kısmı ayırır
    decimal_places = decimal_part[:5]  # İlk 4 haneli kısmı alır
    return decimal_places

temp=0
# Sonuçları yazdırma :
for template_path, similarity in similarities:
    temp=temp+1

    print(temp, template_path, "\nBenzerlik Yüzdeleri: ", get_decimal_places(similarity), "\n")

