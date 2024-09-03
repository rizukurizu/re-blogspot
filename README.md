# blogspot downloader

Script python ini mengunduh semua postingan dari blogspot dan mengubahnya menjadi epub atau pdf, baik dalam bentuk tampilan halaman web maupun rss feed.

Tidak hanya blogspot, jika halaman web manapun yang mengandung rss feed, terutama untuk wordpress, maka script ini dapat mengunduh dalam mode rss.

## Why ?

Layanan online yang ada saat ini harus berbayar, memiliki batasan file, harus menyalin per halaman secara manual, hanya mendukung rss feed, atau hanya mendukung epub. 

Skrip python ini gratis, tidak ada batasan file karena dijalankan di mesin/ip lokal Anda, mengunduh semua halaman/umpan secara otomatis, mendukung rss dan web scraping (beberapa rss blog bersifat pribadi atau hanya satu halaman), mendukung epub dan pdf. Ini juga mendukung tanggal lokal khusus. Yang paling penting: ini adalah kode python sederhana dan Anda dapat dengan bebas memodifikasinya, misalnya warna html kustom, header/footer html tambahan, direktori default... dll :)

## How to setup (Only support python3):

<pre>
git clone https://github.com/limkokhole/blogspot-downloader.git
cd blogspot-downloader/

pip3 install -r requirements_py3.txt 

In ubuntu, run `sudo apt install wkhtmltopdf`.
    
direktori pypub/ berisi berbagai perbaikan bug dan kustomisasi dari <a href=“https://github.com/wcember/pypub”>Pypub</a> yang asli, dan diuji di python 3.8.5

</pre>

## How to run (Only support python3):

    python3 blogspot_downloader.py [url]

    OR

    $ python3 blogspot_downloader.py --help

    usage: blogspot_downloader.py [-h] [-a] [-s] [-d] [-p] [-l LOCALE] [-f FEED]
                                  [-1]
                                  [url]

    Blogspot Downloader

    positional arguments:
      url                   Blogspot url

    optional arguments:
      -h, --help            show this help message and exit
      -a, --all             Display website mode instead of rss feed mode. Only
                            support blogspot website but you can try your luck in
                            other site too
      -s, --single          Download based on provided url year/month instead of
                            entire blog, will ignored in rss feed mode and
                            --print_date
      -d, --print_date      Print main date info without execute anything
      -p, --pdf             Output in PDF instead of EPUB but might failed in some
                            layout
      --js-delay JS_DELAY   Specify delay seconds for -1 -p to have enough time
                            for Javascript to load. Default is 3 seconds.
      -l LOCALE, --locale LOCALE
                            Date translate to desired locale, e.g. -l zh_CN.UTF-8
                            will shows date in chinese
      -f FEED, --feed FEED  Direct pass full rss feed url. e.g. python
                            blogspot_downloader.py http://www.ulduzsoft.com/feed/
                            -f http://www.ulduzsoft.com/feed/. Note that it may
                            not able to get previous rss page in non-blogspot
                            site.
      -1, --one             Scrape url of ANY webpage as single pdf(-p) or epub
      -lo, --log-link-only  print link only log for -f feed, temporary workaround
                            to copy into -1, in case -f feed only retrieve
                            summary.

## Normal usage:

    python3 blogspot_downloader.py [url blogspot] # Unduh blogspot sebagai RSS Feed

    python3 blogspot_downloader.py -1 # Tautan situs web apa pun, atau halaman blogspot lengkap (non-RSS)
    python3 blogspot_downloader.py -1 [url]

    python3 blogspot_downloader.py -lo [url blogspot] # Untuk mendapatkan daftar url blogspot yang dapat disimpan ke /tmp/urls.list secara manual
    python3 blogspot_downloader.py -1 </tmp/urls.list # Unduh daftar url di atas. Saat ini Anda masih perlu menghapus secara manual file keluaran di atas.
    
## Details:

Ini akan menanyakan url blogspot jika Anda tidak memberikan [url] pada opsi perintah.

Gunakan -f rss_feed_url untuk mengunduh dari feed rss, atau -a webpage_url untuk mengunduh dari halaman web. Tips: Anda mungkin beruntung menemukan url feed dengan mengklik kanan pada halaman web dan memilih “View Page Source”, lalu mencari kata kunci “rss”. Perlu diperhatikan bahwa rss_feed / path dapat berdampak pada penyempitan cakupan feed, misalnya https://example.com/2018/05/ dapat mempersempit feed hanya untuk bulan Mei saja, dan https://example.com/2018/ dapat mempersempit feed untuk tahun 2018.

Tidak semua blog bekerja dalam mode -p pdf, Anda akan segera menyadari hal ini jika Anda menemukan duplikasi tata letak untuk beberapa halaman pertama, maka Anda dapat menggunakan ctrl+c untuk menghentikannya. Coba hapus -p atau gunakan -f feed_url sebagai gantinya dalam kasus ini, atau unduh epub saja.

Skrip ini ditujukan untuk Linux dan tidak pernah diuji di Windows. Skrip ini juga tidak dapat dijalankan dalam banyak proses karena akan menghapus file sampah /tmp.

Nama file yang diduplikasi tidak akan diganti tetapi akan ditambahi dengan stempel waktu saat ini.

File ePUB dapat diedit secara manual. Cukup ubah nama menjadi .zip, unzip, edit xhtml, dan (di dalam direktori epub) lakukan `zip -rX ../<nama direktori epub>.epub minetype.txt META-INF/ OEBPS/` untuk mengemas ulang dengan mudah.  Saya merekomendasikan penampil Kchmviewer dan Sigli, tetapi jika tidak bisa dibuka karena mungkin terlalu ketat dalam sintaks xhtml, maka Anda dapat mencoba penampil lain dalam kasus ini (Sigli akan mencoba memperbaiki secara otomatis untuk Anda), dan jangan ragu-ragu untuk mengeluarkan tiket.

## Sample Screenshots:

download non-blogspot site as rss feed in pdf:  

![medium](/images/medium.png?raw=true "download non-blogspot site as rss feed in pdf")  

download blogspot site as rss feed in pdf without file limits:

![google](/images/google.png?raw=true "download blogspot site as rss feed in pdf without file limits")  

download blogspot site as web scraping in ePUB which able to include comments section:

![eat](/images/eat.png?raw=true "download blogspot site as web scraping in ePUB")

download blogspot site as rss feed in ePUB, plus custom locale date in Chinese:  

![locale](/images/locale.png?raw=true "download blogspot site as rss feed in ePUB, plus custom locale")

pdf keep color, while ePUB don't:  

![color](/images/color.png?raw=true "pdf keep color, while ePUB don't")

With --one option, you can paste any webpage link manually to make custom ebook chapter :D (Tip: You can download this extension https://addons.mozilla.org/en-US/firefox/addon/link-gopher to get all the link then copy/paste all in once(No need to paste one by one) into prompt, and don't forget last link need to press Enter. You can also use `< urls.txt` style.). -1 only support single pdf when -p.

![color](/images/perl.png?raw=true "You can even paste any webpage link to create a nice ePUB ebook :D")

## Demonstration video (Click image to play at YouTube): ##
[![watch in youtube](https://i.ytimg.com/vi/B6QzTmMglEo/hqdefault.jpg)](https://www.youtube.com/watch?v=B6QzTmMglEo "Blogspot_downloader")


