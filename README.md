from PIL import Image, ImageDraw, ImageFont
import qrcode

# Datos personales
nombre = "Giulian"
segundo_nombre = "Kenneth"
apellido = "Sirica"
correo = "sirgiukeno@gmail.com"
numero = "691350409"
telegram = "@Giulian_Sirica"
discord = "giulian_sirica"
ruta_foto = "yo.jpg"  # Ruta de la foto

# Crear imagen base para la VCard
ancho, alto = 400, 250
vcard = Image.new('RGB', (ancho, alto), 'white')
dibujo = ImageDraw.Draw(vcard)

# Cargar y añadir la foto
foto = Image.open(ruta_foto)
foto = foto.resize((80, 80))  # Ajusta el tamaño según necesites
vcard.paste(foto, (10, 10))

# Añadir texto
fuente = ImageFont.load_default()
texto = f"{nombre} {segundo_nombre} {apellido}\n{correo}\n{numero}\nTelegram: {telegram}\nDiscord: {discord}"
dibujo.text((100, 10), texto, fill="black", font=fuente)

# Datos personales para el formato vCard
vcard_data = (
    f"BEGIN:VCARD\n"
    f"N:{apellido};{nombre};{segundo_nombre};;\n"
    f"FN:{nombre} {segundo_nombre} {apellido}\n"
    f"EMAIL:{correo}\n"
    f"TEL;TYPE=CELL:{numero}\n"
    f"X-SOCIALPROFILE;TYPE=telegram:user:{telegram}\n"
    f"X-SOCIALPROFILE;TYPE=discord:user:{discord}\n"
    f"END:VCARD"
)

# Crear y configurar el código QR
qr = qrcode.QRCode(
    version=1,
    error_correction=qrcode.constants.ERROR_CORRECT_H,
    box_size=10,
    border=4,
)
qr.add_data(vcard_data)
qr.make(fit=True)

qr_img = qr.make_image(fill_color="black", back_color="white")
qr_img = qr_img.resize((100, 100))  # Tamaño aumentado para mejorar la legibilidad
vcard.paste(qr_img, (ancho-110, alto-110))

# Guardar la VCard
vcard.save("tu_vcard.png")
