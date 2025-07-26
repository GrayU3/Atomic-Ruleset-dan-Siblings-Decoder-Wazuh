📜 Regex Lengkap:

```
\.(\d{4}-\d{2}-\d{2}_\d{2}:\d{2}:\d{2})
(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}\.\d{3})
(\d+)
(INFO|DEBUG|ERROR|WARNING|CRITICAL)
([\w\.]+)
\[req-([a-f0-9\-]+)(?: [^\]]*)?\]
(?: ([0-9\.,]+))?
"(GET|POST|PUT|DELETE) ([^ ]+) HTTP\/([0-9.]+)"
status: (\d{3}) len: (\d+) time: ([0-9\.]+)
```

🔍 Struktur Log Sample:

```
nova-api.log.1.2017-05-16_13:53:08
2017-05-16 00:00:00.008
25746
INFO
nova.osapi_compute.wsgi.server
[req-38101a0b-2096-447d-96ea-a692162415ae ...]
10.11.10.1
"GET /v2/... HTTP/1.1"
status: 200
len: 1893
time: 0.2477829
```

🧩 Penjabaran Tiap Bagian Regex:

1. `\.(\d{4}-\d{2}-\d{2}_\d{2}:\d{2}:\d{2})`
   `\.`= mencocokkan titik literal dalam nama file `(nova-api.log.1.2017...)`

`(\d{4}-\d{2}-\d{2}_\d{2}:\d{2}:\d{2})` = mencocokkan `2017-05-16_13:53:08` (timestamp dari nama file)

`\d{4}` = tahun

`-` = literal dash

`\d{2}` = bulan/hari/jam/menit/detik

`_` = literal underscore

`:` = literal colon

2. `(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}\.\d{3})`
   Ini adalah **timestamp log di dalam isi file**
   Format `YYYY-MM-DD HH:MM:SS.mmm`

`\.` = literal titik sebelum milidetik

3. `(\d+)`
   Mencocokkan **PID (Process ID)** seperti `25746`

4. `(INFO|DEBUG|ERROR|WARNING|CRITICAL)`
   Mencocokkan **level log**

Gunakan **alternatif (|)** untuk mencocokkan salah satu kata

5. `([\w\.]+)`
   Mencocokkan nama modul Python: `nova.osapi_compute.wsgi.server`

`\w` = `[a-zA-Z0-9_]`

`[\w\.]+` = kombinasi kata dan titik

6. `\[req-([a-f0-9\-]+)(?: [^\]]*)?\]`

Mencocokkan `req-id: req-38101a0b-2096-447d-96ea-a692162415ae`

`\[req-` = literal `[req-`

`([a-f0-9\-]+)` = UUID (hexadecimal + `-`)

`(?: [^\]]*)?` = non-capturing group opsional: bisa ada teks tambahan dalam `[req-...]`, misal: project/user ID

`\]` = penutup bracket

7. `(?: ([0-9\.,]+))?`
   IP Address / Hostname → seperti `10.11.10.1`

`(?: ...)?` = seluruh grup opsional

`[0-9\.,]+` = angka, titik, atau koma (kadang bisa lebih dari satu IP)

8. `"(GET|POST|PUT|DELETE)`
   Awal HTTP Request Line

Mencocokkan HTTP Method

9. `([^ ]+)`
   Path URL seperti `/v2/...`

`[^ ]+` = semua karakter kecuali spasi

10. `HTTP\/([0-9.]+)"`
    HTTP Version → `1.1`

`\/` = escape `/`

`[0-9.]+` = angka + titik

11. status: `(\d{3})`

Status HTTP: `200`, `404`, dll

12. `len: (\d+)`
    Panjang (bytes) dari response → `1893`

13. `time: ([0-9\.]+)`
    Waktu respons (dalam detik) → `0.2477829`
