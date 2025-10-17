# Penjelasan Capstone Project Module 1 : Python Fundamental

**Penulis:** Muhammad Arief Munazat

**Modul:** Python Fundamentals – Capstone Project

**Tema:** BMW Premium Selection Showroom/Dealer

**Tujuan dan Ruang Lingkup:**

- Menunjukkan **operasi CRUD, validasi input, pembuatan kode dinamis, dan antarmuka berbasis menu**.
- Mengilustrasikan **sistem sederhana suatu manajemen inventaris showroom/dealer dari BMW Premium Selection (Used Car).**

**Struktur Utama:**

1. **Database** → `bmw_units` (dalam memori)
2. **Fungsi** → CRUD + fungsi pembantu
3. **Program Utama** → Loop menu untuk interaksi pengguna

**Library yang Digunakan:**

- `tabulate` → menampilkan tabel di konsol

# **Penjelasan masing masing komponen:**

---

## 1. **Database (Data Storage / Base Data)**

- Berfungsi sebagai **tempat penyimpanan data utama**.
- Dalam kode kamu, ini berupa:
    
    ```python
    bmw_units = { ... }
    
    ```
    
- Disinilah yang nanti akan menampung seluruh data (produk/unit/mobil)

---

## 2. **Functions (Logic / Business Process)**

- Fungsi adalah **bagian logika pemrosesan**.
- Yang nanti akan menjelaskan **bagaimana data diolah**.
- Contohnya:
    - `show_inventory()` → menampilkan data
    - `add_unit()` → menambah unit baru
    - `update_unit()` → memperbarui data
    - `delete_unit()` → menghapus data
    - `model_abbrevation()` → fungsi bantu untuk membuat singkatan model, untuk mendukung CREAT & UPDATE
- Ini semua disebut **lapisan logika / logic layer** atau suatu **business process layer**.

---

## 3. **Main Program (User Interaction / Control Flow)**

- Bagian ini adalah **pengendali alur program**.
- Berfungsi untuk **berinteraksi dengan pengguna atau user interface**.
- Di sini program menawarkan menu dan memanggil fungsi yang sesuai.
- Disebut juga sebagai **control structure**, **main loop**, atau **application controller**. Ini berfungsi sebagai **pengendali** atau **penggerak** yang mengatur bagaimana fungsi-fungsi CRUD dijalankan.
- Contohnya:

```python
# MAIN PROGRAM
while True:
    print("""
---- BMW PREMIUM SELECTION SALES MENU ----\n
1. Show Available Units
2. Add New Unit
3. Unit Delete / Sold
4. Update Unit
5. Exit Program
""")
```

---

# Penjelasan masing masing unsur atau komponen

## **1️⃣ Import Library**

```python
from tabulate import tabulate

```

**Penjelasan:**

- `tabulate` mengubah list of list menjadi **tabel yang mudah dibaca dengan header**.
- Mendukung berbagai format (`plain`, `grid`, `fancy_grid`, dll.).
- Meningkatkan keterbacaan **tampilan inventaris**.

---

## **2️⃣ Database: BMW Units**

```python
bmw_units = {
    "3 Series": [
        {"code": "320i-SP-2020", "name": "320i Sport", "year": 2020, "price": 650_000_000},
        {"code": "330i-SP-2021", "name": "330i Sport", "year": 2021, "price": 850_000_000}
    ],
    "5 Series": [
        {"code": "520i-LU-2019", "name": "520i Luxury", "year": 2019, "price": 900_000_000},
        {"code": "530e-SP-2022", "name": "530e Sport", "year": 2022, "price": 1_150_000_000}
    ],
    "7 Series": [
        {"code": "730Li-PE-2018", "name": "730Li Pure Excellence", "year": 2018, "price": 1_600_000_000}
    ],
    "X Series": [
        {"code": "X5-XD-2022", "name": "X5 xDrive40i", "year": 2022, "price": 1_750_000_000}
    ],
    "M Series": [
        {"code": "M4-CO-2021", "name": "M4 Coupe Competition", "year": 2021, "price": 2_450_000_000}
    ]
}

```

**Penjelasan**

- `bmw_units` → **dictionary** dengan series BMW sebagai key.
- Setiap key berisi **list of dictionaries** → unit mobil individu.
- Setiap unit memiliki:
    - `code` → identitas unik (misal `"320i-SP-2020"`)
    - `name` → nama mobil lengkap
    - `year` → tahun produksi
    - `price` → dalam Rupiah

**Alasan Struktur:**

- Struktur nested memudahkan **pengelompokan per series**.
- Mudah untuk **operasi CRUD**.
- Meniru **tabel database nyata** dalam memori.

---

## **3️⃣ Fungsi: `show_inventory()` (READ)**

```python
def show_inventory():                               # Menampilkan daftar produk (fungsi READ)
    print("\n---- BMW PREMIUM SELECTION INVENTORY ----\n")
    for series, units in bmw_units.items():
        print(f"🔘 {series}")
        table = []
        for u in units:
            table.append([u["code"], u["name"], u["year"], f"Rp{u['price']:,}"])
        print(tabulate(table, headers=["Code", "Model", "Year", "Price"], tablefmt="fancy_grid"))

```

**Penjelasan Detail**

1. Menampilkan **header inventaris**.
2. Loop melalui **semua series** di `bmw_units`.
3. Membuat **list tabel** untuk setiap series:
    - Setiap baris → `[code, name, year, price diformat]`
    - `f"Rp{u['price']:,}"` → menambahkan pemisah ribuan agar mudah dibaca
4. Menampilkan tabel menggunakan **tabulate**, format `"fancy_grid"`.

**Tujuan:**

- Mengimplementasikan **operasi READ**.
- Memberikan tampilan inventaris yang **mudah dibaca** bagi pengguna.

---

## **4️⃣ Fungsi: `model_abbreviation(model_name)`**

```python
def model_abbreviation(model_name):                 # Membuat alias untuk fungsi CREATE & UPDATE
    mapping = {
        "sport": "SP",
        "luxury": "LU",
        "competition": "CO",
        "pure excellence": "PE",
        "xdrive": "XD",
    }

    lower_name = model_name.lower()                 # Ubah menjadi lowercase
    for key, abbr in mapping.items():               # Cek mapping
        if key in lower_name:
            return abbr

    words = model_name.split()                      # Pisahkan berdasarkan spasi

    if len(words) >= 3:
        return (words[1][0] + words[2][0]).upper()  # Huruf pertama kata ke-2 & ke-3
    elif len(words) == 2:
        return words[1][:2].upper()                 # 2 huruf pertama kata ke-2
    else:                                           # 1 kata → minta input ulang
        new_model = input("Nama model tidak dikenali. Masukkan ulang: ")
        return model_abbreviation(new_model)

```

**Penjelasan**

1. `mapping` → daftar singkatan yang sudah ditentukan.
2. Ubah **model_name menjadi lowercase** agar pencocokan tidak sensitif huruf.
3. Loop mapping → kembalikan singkatan jika ditemukan.
4. Logika fallback:
    - ≥3 kata → huruf pertama kata ke-2 + ke-3
    - 2 kata → 2 huruf pertama kata ke-2
    - 1 kata → minta input ulang secara rekursif
5. Menjamin **kode unik untuk setiap unit**.

---

## **5️⃣ Fungsi: `add_unit()` (CREATE)**

```python
def add_unit():                                     # Menambah unit baru pada masing-masing list series (fungsi CREATE)
    print("\n---- ADD NEW UNIT ----\n")

```

**Langkah 1: Input Series**

```python
    for attempt in range(3):
        series = input("Masukkan series (3 Series / 5 Series / 7 Series / X Series / M Series): ").strip().title()
        if series in bmw_units:
            break
        else:
            print("\n⚠️ Pilih series yang tersedia.")
    else:
        print("\n🚫 Terlalu banyak percobaan salah. Kembali ke menu utama.")
        return

```

- **Loop** → maksimal 3 kali percobaan.
- `strip()` → hapus spasi ekstra
- `title()` → memastikan format sesuai key dictionary
- Kembali ke menu utama jika percobaan habis

---

**Langkah 2: Input Nama Model**

```python
    for attempt in range(3):
        name = input("Masukkan nama lengkap model (misal: 320i Luxury): ").strip()
        parts = name.split()
        has_number = any(any(c.isdigit() for c in p) for p in parts)
        if len(parts) >= 2 and has_number:
            name = parts[0] + " " + " ".join(p.title() for p in parts[1:])
            break
        else:
            print("⚠️ Informasi tidak lengkap. Harus menyertakan kode model dan tipe (misal: 320i Luxury).")
    else:
        print("\n🚫 Terlalu banyak percobaan salah. Kembali ke menu utama.")
        return

```

**Penjelasan:**

- Harus menyertakan **kode model (angka)** dan **tipe model**.
- Ubah kata tipe menjadi **title case**.
- Memastikan **input valid sebelum lanjut**.

---

**Langkah 3: Input Tahun**

```python
    for attempt in range(3):
        year = input("Masukkan tahun (4 digit): ").strip()
        if year.isdigit() and len(year) == 4:
            break
        else:
            print("⚠️ Masukkan angka 4 digit (misal: 2020).")
    else:
        print("\n🚫 Terlalu banyak percobaan salah. Kembali ke menu utama.")
        return

```

- Harus **numeric dan 4 digit**.
- Maksimal 3 percobaan → kembali ke menu utama jika salah.

---

**Langkah 4: Input Harga**

```python
    for attempt in range(3):
        price_input = input("Masukkan harga (angka saja): ").strip().replace(".", "").replace(",", "")
        if price_input.isdigit():
            price = int(price_input)
            break
        else:
            print("⚠️ Harga tidak valid. Masukkan angka saja (misal: 350000000).")
    else:
        print("\n🚫 Terlalu banyak percobaan salah. Kembali ke menu utama.")
        return

```

- Hapus format (`.` atau `,`) sebelum diubah menjadi **integer**.
- Memastikan **harga berupa angka**.

---

**Langkah 5: Generate Kode Unik**

```python
    parts = name.split()
    model_code = ""
    for p in parts:
        if any(c.isdigit() for c in p):
            model_code = p
            break

    abbrev = model_abbreviation(name)
    code = f"{model_code}-{abbrev}-{year}"

```

- Mendeteksi **nomor model** dari nama.
- Menggunakan **fungsi abbreviation**.
- Membuat kode unik **model-abbreviation-tahun**.

---

**Langkah 6: Simpan Unit Baru**

```python
    new_unit = {"code": code, "name": name, "year": int(year), "price": price}
    bmw_units[series].append(new_unit)
    print(f"\n✅ {name} berhasil ditambahkan ke {series} dengan kode {code}.")

```

- Menambahkan ke list series yang sesuai.
- Menampilkan konfirmasi.

## **6️⃣ Fungsi: `delete_unit()` (DELETE)**

```python
def delete_unit(): 
    print("\n---- DELETE UNIT or SOLD ----\n")
    show_inventory()

```

- Menampilkan header untuk menandai **mode penghapusan**.
- Memanggil `show_inventory()` → pengguna dapat melihat **stok saat ini**.

---

**Langkah 1: Setup Percobaan**

```python
    max_attempts = 3  
    attempt = 0

```

- Menentukan **maksimum percobaan input salah**.
- `attempt` → melacak percobaan saat ini.

---

**Langkah 2: Loop Hingga Kode Valid atau Maks Percobaan**

```python
    while attempt < max_attempts:   
        code = input("Enter the code of the unit to delete: ").strip()
        found = False

```

- **Loop** berlangsung hingga kode valid ditemukan atau percobaan habis.
- `found` → boolean untuk menandai apakah kode ada di database.

---

**Langkah 3: Cari Unit Berdasarkan Kode**

```python
        for series, units in bmw_units.items():
            for u in units:
                if u["code"] == code:
                    confirm = input(f"Are you sure to delete {u['name']} from {series}? (y/n): ").lower()
                    if confirm == "y":
                        units.remove(u)
                        print(f"\n✅ {u['name']} has been removed from {series}.")
                    else:
                        print("\n❌ Deletion cancelled.")
                    return

```

- Loop nested **series → units** untuk mencari kecocokan.
- Meminta **konfirmasi** sebelum menghapus.
- `units.remove(u)` → menghapus dictionary dari list.
- `return` → keluar fungsi langsung setelah hapus atau batal.

---

**Langkah 4: Jika Tidak Ditemukan & Retry**

```python
        if not found:
                attempt += 1
                print(f"\n⚠️  No unit found with that code. Attempt {attempt}/{max_attempts}.")

                if attempt < max_attempts:
                    choice = input("Type 'y' to return to main menu or 'enter' to try again): ").lower()
                    if choice == "y":
                        print("\n↩️ Returning to main menu.")
                        return
                    else:
                        print("\n🔁 Please try again.\n")
                else:
                    print("\n🚫 Maximum attempts reached. No unit found with that code.")
                    return

```

- Jika **kode tidak ditemukan**, `attempt` bertambah.
- Memberi opsi **kembali ke menu utama** atau mencoba lagi.
- Jika `max_attempts` tercapai → keluar fungsi.

---

## **7️⃣ Fungsi: `update_unit()` (UPDATE)**

```python
def update_unit():         
    print("\n---- UPDATE UNIT ----\n")
    show_inventory()

```

- Menampilkan **header update**.
- Menunjukkan **inventaris saat ini**.

---

**Langkah 1: Setup Percobaan**

```python
    max_attempts = 3            
    attempt = 0

```

- Sama seperti delete → **maksimal 3 percobaan**.

---

**Langkah 2: Loop Hingga Kode Valid**

```python
    while attempt < max_attempts:
        code = input("Enter the code of the unit to update: ").strip()
        found = False

```

- Menunggu input kode unit yang ingin diperbarui.
- `found` menandai apakah unit ada.

---

**Langkah 3: Cari Unit**

```python
        for series, units in bmw_units.items():
            for u in units:
                if u["code"] == code:
                    found = True
                    print(f"\n✅ Found {u['name']} in {series}")
                    print("Leave a field blank if you don't want to change it.")

```

- Loop nested untuk menemukan unit.
- `found=True` → unit ada.
- Memberi tahu pengguna bisa **melewatkan field** dengan membiarkannya kosong.

---

**Langkah 4: Update Nama Model**

```python
                    for name_attempt in range(3):
                        new_name = input(f"Enter new model name (current: {u['name']}): ").strip().title()
                        if new_name == "":
                            break
                        parts = new_name.split()
                        if len(parts) < 2 or not any(any(c.isdigit() for c in p) for p in parts):
                            print(f"⚠️ Enter new model name. Attempt {name_attempt + 1}/3")
                            continue
                        u["name"] = new_name.title()
                        model_code = next((p for p in parts if any(c.isdigit() for c in p)), "")
                        abbrev = model_abbreviation(new_name)
                        u["code"] = f"{model_code}-{abbrev}-{u['year']}"
                        break

```

- Memberi **3 percobaan** input nama model valid.
- Input kosong → **skip update**.
- Memastikan **minimal 2 kata + angka pada kode model**.
- Memperbarui **`name`** dan **`code`** menggunakan singkatan.

---

**Langkah 5: Update Tahun**

```python
                    for year_attempt in range(3):
                        new_year = input(f"Enter new year (current: {u['year']}): ").strip()
                        if new_year == "":
                            break
                        if not (new_year.isdigit() and len(new_year) == 4):
                            print(f"⚠️ Invalid year. Must be 4 digits. Attempt {year_attempt + 1}/3")
                            continue
                        u["year"] = int(new_year)
                        parts = u["code"].split("-")
                        parts[-1] = str(new_year)
                        u["code"] = "-".join(parts)
                        break

```

- 3 percobaan → **harus numeric 4 digit**.
- Memperbarui `year` dan **segmen terakhir dari `code`**.

---

**Langkah 6: Update Harga**

```python
                    for price_attempt in range(3):
                        new_price = input(f"Enter new price (current: Rp{u['price']:,}): ").strip()
                        if new_price == "":
                            break
                        if not new_price.isdigit():
                            print(f"⚠️ Price must be numeric. Attempt {price_attempt + 1}/3")
                            continue
                        u["price"] = int(new_price)
                        break

```

- 3 percobaan → **harga harus numeric**.
- Kosong → **skip update**.

---

**Langkah 7: Konfirmasi Akhir**

```python
                    print(f"\n✅ {u['name']} has been successfully updated!")
                    return

```

- Konfirmasi update dan **keluar dari fungsi**.

---

**Langkah 8: Jika Tidak Ditemukan**

```python
        if not found:
            attempt += 1
            print(f"\n⚠️  No unit found with that code. Attempt {attempt}/{max_attempts}")
            if attempt < max_attempts:
                choice = input("Type 'y' to return to main menu or 'enter' to try again: ").lower()
                if choice == "y":
                    print("\n↩️ Returning to main menu")
                    return
                else:
                        print("\n🔁 Please try again.\n")
            else:
                print("🚫 Maximum attempts reached. No unit found with that code.")
                return

```

- Sama seperti `delete_unit()` → retry diperbolehkan, ada opsi **kembali ke menu**.

---

## **8️⃣ Program Utama: Loop Menu**

```python
while True:
    print("""
---- BMW PREMIUM SELECTION SALES MENU ----\n
1. Show Available Units
2. Add New Unit
3. Unit Delete / Sold
4. Update Unit
5. Exit Program
""")

    choice = input("Select menu (1-5): ")

```

- Loop tak hingga → **program terus berjalan hingga exit**.
- Menampilkan **pilihan menu** ke pengguna.
- Meminta input pilihan (1–5).

---

**Langkah Pilihan Menu**

```python
    if choice == "1":
        show_inventory()
    elif choice == "2":
        add_unit()
    elif choice == "3":
        delete_unit()
    elif choice == "4":
        update_unit()
    elif choice == "5":
        print("Thank you for using BMW Premium Selection Sales System!")
        break
    else:
        print("\n❌ Invalid choice. Please try again.")
        continue

```

- 1 → tampilkan inventory (READ)
- 2 → tambah unit (CREATE)
- 3 → hapus unit (DELETE)
- 4 → update unit (UPDATE)
- 5 → keluar loop → akhir program
- Input tidak valid → **coba ulang menu**

---

# Kesimpulan

Setiap bagian dalam program ini punya fungsi CRUD:

| CRUD | Fungsi | Tujuan |
| --- | --- | --- |
| **Create** | `add_unit()` | Menambah mobil baru |
| **Read** | `show_inventory()` | Menampilkan seluruh data |
| **Update** | `update_unit()` | Mengubah data mobil |
| **Delete** | `delete_unit()` | Menghapus data mobil |

---

Dan fungsi-fungsi tersebut harus di buat dahulu, setelah nya baru dijalankan dengan while true agar program berjalan dengan memakai fungsi-fungsi yang telah di buat atau didefinisikan menggunakan regular function sebelum nya.

Sekian dari penjelasan capstone project pada module 1, terkait basic python fundamenta ini.

Terima kasih!
