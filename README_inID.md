# [Panduan Gaya Penulisan Elixir][Elixir Style Guide]

## Daftar Isi

* __[Pendahuluan](#pendahuluan)__
* __[Tentang](#tentang)__
* __[Formatting](#formatting)__
  * [*Whitespace*](#whitespace)
  * [Indentasi](#indentation)
  * [Tanda Kurung](#parentheses)
* __[Panduan](#the-guide)__
  * [Ekspresi](#expressions)
  * [Penamaan](#naming)
  * [Komentar](#comments)
    * [Anotasi Komentar](#comment-annotations)
  * [`Modules`](#modules)
  * [Dokumentasi](#documentation)
  * [*Typespecs*](#typespecs)
  * [*Structs*](#structs)
  * [*Exceptions*](#exceptions)
  * [*Collections*](#collections)
  * [`Strings`](#strings)
  * _Regular Expressions_
  * [*Metaprogramming*](#metaprogramming)
  * [Pengujian](#testing)
* __[Bahan Bacaan](#resources)__
  * [Panduan Gaya Penulisan Lain](#alternative-style-guides)
  * [Kakas](#tools)
* __[Ikut Terlibat](#getting-involved)__
  * [Berkontribusi](#contributing)
  * [Spread the Word](#spread-the-word)
* __[Penyalinan](#copying)__
  * [Lisensi](#license)
  * [Atribusi](#attribution)

## Pendahuluan

> *Liquid architecture. It's like jazz — you improvise, you work together, you
> play off each other, you make something, they make something.*
>
> —Frank Gehry

Gaya penulisan itu penting.
[Elixir] memiliki berbagai gaya penulisan. Namun seperti semua bahasa, penulisannya dapat terkesan kaku.
Jangan mengekang gaya penulisan.

## Tentang

Ini merupakan panduan penulisan komunitas untuk [bahasa pemrograman Elixir][Elixir].
Silakan mengajukan *pull requests* dan saran, dan menjadi bagian dari komunitas Elixir.

Apabila Anda ingin berkontribusi pada proyek lain, silakan liat
[*situs Hex package manager*][Hex].

<a name="translations"></a>
Terjemahan panduan ini tersedia dalam bahasa berikut:

* [bahasa Cina yang disederhanakan][Chinese Simplified]
* [bahasa tradisional Cina][Chinese Traditional]
* [bahasa Perancis][French]
* [bahasa Jepang][Japanese]
* [bahasa Korea][Korean]
* [bahasa Portugis][Portuguese]
* [bahasa Spanyol][Spanish]
* [bahasa Indonesia][Indonesian]

## *Formatting*

Elixir v1.6 memperkenalkan sebuah [*Code Formatter*][Code Formatter] and [*Mix format*][Mix format] task.
The formatter should be preferred for all new projects and source code.

Aturan berikut telah diterapkan secara otomatis oleh *code formatter* namun ditampilkan di sini sebagai contoh yang dianjurkan.

### *Whitespace*

* <a name="trailing-whitespace"></a>
  Hindari *trailing whitespace*.
  <sup>[[tautan](#trailing-whitespace)]</sup>

* <a name="newline-eof"></a>
  Akhiri berkas dengan baris baru.
  <sup>[[tautan](#newline-eof)]</sup>

* <a name="line-endings"></a>
  Gunakan akhir baris *Unix-style* (\*pengguna BSD/Solaris/Linux/OSX sudah tercakup secara bawaan, pengguna Windows perlu berhati-hati).
  <sup>[[tautan](#line-endings)]</sup>

* <a name="autocrlf"></a>
  Apabila Anda menggunakan Git, Anda mungkin dapat menambahkan konfigurasi berikut untuk mencegah proyek Anda dari akhir baris *Windows-style* dengan:
  <sup>[[tautan](#autocrlf)]</sup>

  ```sh
  git config --global core.autocrlf true
  ```

* <a name="line-length"></a>
  Jumlah maksimum karakter dalam 1 baris adalah 98 karakter. Jika tidak, tentukan  opsi `:line_length` pada berkas `.formatter.exs` Anda.
  <sup>[[tautan](#line-length)]</sup>

* <a name="spaces"></a>
  Gunakan spasi di antara operator, setelah koma, titik dua, dan titik koma.
  Jangan gunakan spasi di antara tanda baca berpasanganm seperti tanda kurung siku, tanda kurung, dll.
  *Whitespace* kemungkinan tidak relevan dengan Elixir *runtime* namun ini adalah penggunaan yang tepat agar kode dapat mudah dibaca.
  <sup>[[tautan](#spaces)]</sup>

  ```elixir
  sum = 1 + 2
  {a, b} = {2, 3}
  [first | rest] = [1, 2, 3]
  Enum.map(["one", <<"two">>, "three"], fn num -> IO.puts(num) end)
  ```

* <a name="no-spaces"></a>
  Jangan gunakan spasi setelah operator bukan-kata yang hanya memiliki 1 argumen; atau di antara operator *range*.
  <sup>[[tautan](#no-spaces)]</sup>

  ```elixir
  0 - 1 == -1
  ^pinned = some_func()
  5 in 1..10
  ```

* <a name="def-spacing"></a>
  Gunakan baris kosong di antara `def` untuk memisahkan fungsi menjadi paragraf logis. <sup>[[tautan](#def-spacing)]</sup>

  ```elixir
  def some_function(some_data) do
    some_data |> other_function() |> List.first()
  end

  def some_function do
    result
  end

  def some_other_function do
    another_result
  end

  def a_longer_function do
    one
    two

    three
    four
  end
  ```

* <a name="defmodule-spacing"></a>
  Jangan letakkan baris kosong setelah `defmodule`.
  <sup>[[tautan](#defmodule-spacing)]</sup>

* <a name="long-dos"></a>
  Jika kepala fungsi dan klausa `do:` terlalu panjang untuk masuk ke dalam satu baris, gunakan
  `do:` pada baris baru, berikan indentasi 1 level lebih banyak daripada baris sebelumnya.
  <sup>[[tautan](#long-dos)]</sup>

  ```elixir
  def some_function([:foo, :bar, :baz] = args),
    do: Enum.map(args, fn arg -> arg <> " is on a very long line!" end)
  ```

  Ketika klausa `do:` berada di awal baris, perlakukan ini sebagai fungsi *multiline*
  dengan memisahkannya dengan baris kosong.

  ```elixir
  # tidak dianjurkan
  def some_function([]), do: :empty
  def some_function(_),
    do: :very_long_line_here

  # dianjurkan
  def some_function([]), do: :empty

  def some_function(_),
    do: :very_long_line_here
  ```

* <a name="add-blank-line-after-multiline-assignment"></a>
  Gunakan baris kosong setelah *multiline assignment* sebagai tanda bahwa *assignment* telah berakhir.
  <sup>[[tautan](#add-blank-line-after-multiline-assignment)]</sup>

  ```elixir
  # tidak dianjurkan
  some_string =
    "Hello"
    |> String.downcase()
    |> String.trim()
  another_string <> some_string

  # dianjurkan
  some_string =
    "Hello"
    |> String.downcase()
    |> String.trim()

  another_string <> some_string
  ```

  ```elixir
  # tidak dianjurkan
  something =
    if x == 2 do
      "Hi"
    else
      "Bye"
    end
  String.downcase(something)

  # dianjurkan
  something =
    if x == 2 do
      "Hi"
    else
      "Bye"
    end

  String.downcase(something)
  ```

* <a name="multiline-enums"></a>
  Jika `list`, `map`, or `struct` terdiri atas beberapa baris, letakkan tiap elemen, termasuk kurung buka dan tutup, pada barisnya sendiri.
  Berikan 1 level indentasi pada tiap elemen namun tidak untuk tanda kurung.
  <sup>[[tautan](#multiline-enums)]</sup>

  ```elixir
  # tidak dianjurkan
  [:first_item, :second_item, :next_item,
  :final_item]

  # dianjurkan
  [
    :first_item,
    :second_item,
    :next_item,
    :final_item
  ]
  ```

* <a name="multiline-list-assign"></a>
  Ketika meng-*assign* `list`, `map`, atau `struct`, letakkan kurung buka pada baris yang sama dengan *assignment*.
  <sup>[[tautan](#multiline-list-assign)]</sup>

  ```elixir
  # tidak dianjurkan
  list =
  [
    :first_item,
    :second_item
  ]

  # dianjurkan
  list = [
    :first_item,
    :second_item
  ]
  ```

* <a name="multiline-case-clauses"></a>
  Jika klausa `case` or `cond` membutuhkan lebih dari 1 baris (karena panjang baris,
  banyak ekspresi pada badan klausa, dll.), gunakan sintaksis *multiline* pada semua klausanya dan pisahkan dengan baris kosong.
  <sup>[[tautan](#multiline-case-clauses)]</sup>

  ```elixir
  # tidak dianjurkan
  case arg do
    true -> IO.puts("ok"); :ok
    false -> :error
  end

  # tidak dianjurkan
  case arg do
    true ->
      IO.puts("ok")
      :ok
    false -> :error
  end

  # dianjurkan
  case arg do
    true ->
      IO.puts("ok")
      :ok

    false ->
      :error
  end
  ```

* <a name="comments-above-line"></a>
  Letakkan komentar di atas baris yang mereka komentari.
  <sup>[[tautan](#comments-above-line)]</sup>

  ```elixir
  String.first(some_string) # tidak dianjurkan

  # dianjurkan
  String.first(some_string)
  ```

* <a name="comment-leading-spaces"></a>
  Gunakan satu spasi di antara awal karakter `#` dari komentar dan teks komentarnya.
  <sup>[[tautan](#comment-leading-spaces)]</sup>

  ```elixir
  #tidak dianjurkan
  String.first(some_string)

  # dianjurkan
  String.first(some_string)
  ```

### Indentasi

* <a name="with-clauses"></a>
  Berikan indentasi dan luruskan klausa `with`.
  Letakkan klausa `do:` pada baris baru, luruskan dengan klausa sebelumnya.
  <sup>[[tautan](#with-clauses)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, my_var} <- fetch(opts, :my_var),
       do: {:ok, foo, my_var}
  ```

* <a name="with-else"></a>
  Jika ekspresi `with` memiliki *multiline* blok `do` atau memiliki opsi `else`, gunakan sintaksis *multiline*
  <sup>[[tautan](#with-else)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, my_var} <- fetch(opts, :my_var) do
    {:ok, foo, my_var}
  else
    :error ->
      {:error, :bad_arg}
  end
  ```

### Tanda Kurung

* <a name="parentheses-pipe-operator"></a>
  Gunakan tanda kurung untuk fungsi satu *arity* ketika menggunakan operator `pipe` (`|>`).
  <sup>[[tautan](#parentheses-pipe-operator)]</sup>

  ```elixir
  # tidak dianjurkan
  some_string |> String.downcase |> String.trim

  # dianjurkan
  some_string |> String.downcase() |> String.trim()
  ```

* <a name="function-names-with-parentheses"></a>
  Jangan letakkan spasi di antara nama fungsi dan kurung buka.
  <sup>[[tautan](#function-names-with-parentheses)]</sup>

  ```elixir
  # tidak dianjurkan
  f (3 + 2)

  # dianjurkan
  f(3 + 2)
  ```

* <a name="function-calls-and-parentheses"></a>
  Gunakan tanda kurung pada pemanggilan fungsi, terkhusus jika menggunakan operator `pipe`.
  <sup>[[tautan](#function-calls-and-parentheses)]</sup>

  ```elixir
  # tidak dianjurkan
  f 3

  # dianjurkan
  f(3)

  # tidak dianjurkan dan ini akan terbaca sebagai rem(2, (3 |> g)), yang merupakan hal yang tidak kamu inginkan.
  2 |> rem 3 |> g

  # dianjurkan
  2 |> rem(3) |> g()
  ```

* <a name="keyword-list-brackets"></a>
  Hilangkan kurung siku dari *keyword lists* apabila opsional.
  <sup>[[tautan](#keyword-list-brackets)]</sup>

  ```elixir
  # tidak dianjurkan
  some_function(foo, bar, [a: "baz", b: "qux"])

  # dianjurkan
  some_function(foo, bar, a: "baz", b: "qux")
  ```

## Panduan

Panduan berikut mungkin tidak diterapkan pada *code formatter* namun ini adalah praktik yang umumnya disukai.

### Ekspresi

* <a name="single-line-defs"></a>
  Gabungkan `def` *single-line* pada fungsi yang sama namun pisahkan dari `def` *multiline* dengan baris kosong.
  <sup>[[tautan](#single-line-defs)]</sup>

  ```elixir
  def some_function(nil), do: {:error, "No Value"}
  def some_function([]), do: :ok

  def some_function([first | rest]) do
    some_function(rest)
  end
  ```

* <a name="multiple-function-defs"></a>
  Jika Anda memiliki lebih dari satu `def` *multiline*, jangan gunakan `def` *single-line*.
  <sup>[[tautan](#multiple-function-defs)]</sup>

  ```elixir
  def some_function(nil) do
    {:error, "No Value"}
  end

  def some_function([]) do
    :ok
  end

  def some_function([first | rest]) do
    some_function(rest)
  end

  def some_function([first | rest], opts) do
    some_function(rest, opts)
  end
  ```

* <a name="pipe-operator"></a>
  Gunakan operator `pipe` untuk merantai fungsi.
  <sup>[[tautan](#pipe-operator)]</sup>

  ```elixir
  # tidak dianjurkan
  String.trim(String.downcase(some_string))

  # dianjurkan
  some_string |> String.downcase() |> String.trim()

  # Multiline pipelines tidak diberi indentasi lebih
  some_string
  |> String.downcase()
  |> String.trim()

  # Multiline pipelines di sisi kanan dari pencocokan pola
  # perlu dikasih indentasi pada baris baru
  sanitized_string =
    some_string
    |> String.downcase()
    |> String.trim()
  ```

  Meskipun ini merupakan metode yang dianjurkan, perlu diperhatikan bahwa menyalin *multiline pipelines* ke `IEx` dapat menyebabkan eror sintaksis. Hal ini disebabkan `IEx` akan mengevaluasi baris pertama tanpa menyadari bahwa baris selanjutnya memiliki `pipeline`. 
  Untuk mencegah ini, Anda dapat membungkus kode dengan tanda kurung.

* <a name="avoid-single-pipelines"></a>
  Hindari hanya satu penggunaan operator `pipe`.
  <sup>[[tautan](#avoid-single-pipelines)]</sup>

  ```elixir
  # tidak dianjurkan
  some_string |> String.downcase()

  System.version() |> Version.parse()

  # dianjurkan
  String.downcase(some_string)

  Version.parse(System.version())
  ```

* <a name="bare-variables"></a>
  Gunakan variabel *bare* pada awal dari rantai fungsi
  <sup>[[tautan](#bare-variables)]</sup>

  ```elixir
  # tidak dianjurkan
  String.trim(some_string) |> String.downcase() |> String.codepoints()

  # dianjurkan
  some_string |> String.trim() |> String.downcase() |> String.codepoints()
  ```

* <a name="fun-def-parentheses"></a>
  Gunakan tanda kurung ketika `def` memiliki argumen dan hilangkan ketika tidak memilikinya.
  <sup>[[tautan](#fun-def-parentheses)]</sup>

  ```elixir
  # tidak dianjurkan
  def some_function arg1, arg2 do
    # body omitted
  end

  def some_function() do
    # body omitted
  end

  # dianjurkan
  def some_function(arg1, arg2) do
    # body omitted
  end

  def some_function do
    # body omitted
  end
  ```

* <a name="do-with-single-line-if-unless"></a>
  Gunakan `do:` untuk pernyataan `if/unless` *single-line*
  <sup>[[tautan](#do-with-single-line-if-unless)]</sup>

  ```elixir
  # dianjurkan
  if some_condition, do: # some_stuff
  ```

* <a name="unless-with-else"></a>
  Jangan gunakan `unless` dengan `else`.
  Tulis kembali kode berikut dengan kasus positif dahulu.
  <sup>[[tautan](#unless-with-else)]</sup>

  ```elixir
  # tidak dianjurkan
  unless success do
    IO.puts('failure')
  else
    IO.puts('success')
  end

  # dianjurkan
  if success do
    IO.puts('success')
  else
    IO.puts('failure')
  end
  ```

* <a name="true-as-last-condition"></a>
  Gunakan `true` sebagai kondisi terakhir dari `cond` ketika Anda membutuhkan klausa yang selalu cocok.
  <sup>[[tautan](#true-as-last-condition)]</sup>

  ```elixir
  # tidak dianjurkan
  cond do
    1 + 2 == 5 ->
      "Nope"

    1 + 3 == 5 ->
      "Uh, uh"

    :else ->
      "OK"
  end

  # dianjurkan
  cond do
    1 + 2 == 5 ->
      "Nope"

    1 + 3 == 5 ->
      "Uh, uh"

    true ->
      "OK"
  end
  ```

* <a name="parentheses-and-functions-with-zero-arity"></a>
  Gunakan tutup kurung untuk pemanggilan fungsi dengan *arity* nol sehingga ia dapat dibedakan dari variabel.
  Mulai dari Elixir 1.4, *compiler* akan mengingatkan Anda apabila terdapat ambiguitas.
  <sup>[[tautan](#parentheses-and-functions-with-zero-arity)]</sup>

  ```elixir
  defp do_stuff, do: ...

  # tidak dianjurkan
  def my_func do
    # ini itu variabel atau pemanggilan fungsi?
    do_stuff
  end

  # dianjurkan
  def my_func do
    # ini tentu saja pemanggilan fungsi
    do_stuff()
  end
  ```

### Penamaan

Panduan ini mengikuti [Konvensi Penamaan][Naming Conventions] dari dokumentasi Elixir
termasuk penggunaan `snake_case` dan `CamelCase` untuk mendeskripsikan aturan huruf kapital.

* <a name="snake-case"></a>
  Gunakan `snake_case` untuk `atoms`, fungsi, dan variabel.
  <sup>[[tautan](#snake-case)]</sup>

  ```elixir
  # tidak dianjurkan
  :"some atom"
  :SomeAtom
  :someAtom

  someVar = 5

  def someFunction do
    ...
  end

  # dianjurkan
  :some_atom

  some_var = 5

  def some_function do
    ...
  end
  ```

* <a name="camel-case"></a>
  Gunakan `CamelCase` untuk `modules` (gunakan huruf kapital untuk akronim, seperti HTTP, RFC, XML).
  <sup>[[tautan](#camel-case)]</sup>

  ```elixir
  # tidak dianjurkan
  defmodule Somemodule do
    ...
  end

  defmodule Some_Module do
    ...
  end

  defmodule SomeXml do
    ...
  end

  # dianjurkan
  defmodule SomeModule do
    ...
  end

  defmodule SomeXML do
    ...
  end
  ```

* <a name="predicate-function-trailing-question-mark"></a>
  Fungsi yang mengembalikan `boolean` (`true` atau `false`) harus dinamai dengan akhiran tanda tanya.
  <sup>[[tautan](#predicate-function-trailing-question-mark)]</sup>

  ```elixir
  def cool?(var) do
    String.contains?(var, "cool")
  end
  ```

* <a name="predicate-function-is-prefix"></a>
  Pengecekan `boolean` yang dapat digunakan pada klausa `guard` harus dinamai dengan prefiks `is_`.
  Untuk daftar ekspresi yang dibolehkan, dapat dilihat pada dokumentasi [`Guard`][Guard Expressions].
  <sup>[[tautan](#predicate-function-is-prefix)]</sup>

  ```elixir
  defguard is_cool(var) when var == "cool"
  defguard is_very_cool(var) when var == "very cool"
  ```

* <a name="private-functions-with-same-name-as-public"></a>
  Fungsi pribadi seharusnya tidak memiliki nama yang sama dengan fungsi publik.
  Dan juga, tidak dianjurkan untuk menggunakan pola `def name` dan `defp do_name`.

  Biasanya orang dapat mencari nama yang lebih deskriptif yang memfokuskan pada perbedaannya.
  <sup>[[tautan](#private-functions-with-same-name-as-public)]</sup>

  ```elixir
  def sum(list), do: sum_total(list, 0)

  # fungsi pribadi
  defp sum_total([], total), do: total
  defp sum_total([head | tail], total), do: sum_total(tail, head + total)
  ```

### Komentar

* <a name="expressive-code"></a>
  Tulis kode dengan ekspresif dan coba menyampaikan tujuan dari program dengan *control-flow*, struktur, dan penamaan.
  <sup>[[tautan](#expressive-code)]</sup>

* <a name="comment-grammar"></a>
  Gunakan huruf kapital pada komentar yang lebih dari 1 kata dan gunakan tanda baca pada kalimat.
  Gunakan [satu spasi][Sentence Spacing] setelah titik.
  <sup>[[tautan](#comment-grammar)]</sup>

  ```elixir
  # tidak dianjurkan
  # komentar huruf kecil berikut kehilangan tanda baca

  # dianjurkan
  # Contoh penggunaan huruf kapital
  # Gunakan tanda baca untuk kalimat lengkap.
  ```

* <a name="comment-line-length"></a>
  Batasi komentar Anda dengan 100 karakter.
  <sup>[[tautan](#comment-line-length)]</sup>

#### Anotasi Komentar

* <a name="annotations"></a>
  Anotasi seharusnya ditulis di atas kode yang relevan.
  <sup>[[tautan](#annotations)]</sup>

* <a name="annotation-keyword"></a>
  Kata kunci anotasi ditulis dengan huruf kapital dan diikuti dengan titik dua atau spasi, lalu berikan catatan.
  <sup>[[tautan](#annotation-keyword)]</sup>

  ```elixir
  # TODO: Deprecate in v1.5.
  def some_function(arg), do: {:ok, arg}
  ```

* <a name="exceptions-to-annotations"></a>
  Untuk kasus ketika permasalahan terlihat sangat jelas dan dokumentasi akan membuat redundan, Anda dapat menghilangkan catatan pada anotasi.
  Penggunaan ini hanya untuk pengecualian saja bukan menjadi aturan.
  <sup>[[tautan](#exceptions-to-annotations)]</sup>

  ```elixir
  start_task()

  # FIXME
  Process.sleep(5000)
  ```

* <a name="todo-notes"></a>
  Gunakan `TODO` untuk menandai fitur yang hilang atau fungsionalitas yang seharusnya ada di kemudian hari.
  <sup>[[tautan](#todo-notes)]</sup>

* <a name="fixme-notes"></a>
  Gunakan `FIXME` untuk menandai kode yang salah yang perlu diperbaiki secepatnya.
  <sup>[[tautan](#fixme-notes)]</sup>

* <a name="optimize-notes"></a>
  Gunakan `OPTIMIZE` untuk menandai kode yang lambat atau tidak efisien yang mungkin menyebabkan masalah kinerja.
  <sup>[[tautan](#optimize-notes)]</sup>

* <a name="hack-notes"></a>
  Gunakan `HACK` untuk menandai *code smells* yang menggunakan standar yang dipertanyakan dan perlu direfaktor.
  <sup>[[tautan](#hack-notes)]</sup>

* <a name="review-notes"></a>
  Gunakan `REVIEW` untuk menandai code yang perlu dikaji lebih lanjut untuk memastikan bahwa kode berfungsi sebagaimana dimaksud.
  For example: `REVIEW: Are we sure this is how the client does X currently?`
  <sup>[[tautan](#review-notes)]</sup>

* <a name="custom-keywords"></a>
  Gunakan kata kunci anotasi khusus jika dirasa cocok namun pastikan Anda menambahkannya pada dokumentasi proyek atau `README`.
  <sup>[[tautan](#custom-keywords)]</sup>

### `Modules`

* <a name="one-module-per-file"></a>
  Gunakan satu `module` ber berkas kecuali `module` tersebut hanya digunakan secara internal oleh `module` lain (seperti pengujian).
  <sup>[[tautan](#one-module-per-file)]</sup>

* <a name="underscored-filenames"></a>
  Gunakan `snake_case` untuk penamaan file dan `CamelCase` untuk nama `module`.
  <sup>[[tautan](#underscored-filenames)]</sup>

  ```elixir
  # berkas ini bernaam some_module.ex

  defmodule SomeModule do
  end
  ```

* <a name="module-name-nesting"></a>
  Wakili tiap level dalam nama `module` sebagai direktori.
  <sup>[[tautan](#module-name-nesting)]</sup>

  ```elixir
  # berkas ini bernama parser/core/xml_parser.ex

  defmodule Parser.Core.XMLParser do
  end
  ```

* <a name="module-attribute-ordering"></a>
  Berikut merupakan daftar atribut, arahan, dan makro `module`:
  <sup>[[tautan](#module-attribute-ordering)]</sup>

  1. `@moduledoc`
  1. `@behaviour`
  1. `use`
  1. `import`
  1. `require`
  1. `alias`
  1. `@module_attribute`
  1. `defstruct`
  1. `@type`
  1. `@callback`
  1. `@macrocallback`
  1. `@optional_callbacks`
  1. `defmacro`, `defmodule`, `defguard`, `def`, etc.

  Tambahkan baris kosong untuk tiap grup dan sortir berdasarkan abjad.
  Berikut merupakan contoh bagaimana seharusnya Anda menyortir di dalam sebuah `module`:

  ```elixir
  defmodule MyModule do
    @moduledoc """
    Contoh modul
    """

    @behaviour MyBehaviour

    use GenServer

    import Something
    import SomethingElse

    require Integer

    alias My.Long.Module.Name
    alias My.Other.Module.Example

    @module_attribute :foo
    @other_attribute 100

    defstruct [:name, params: []]

    @type params :: [{binary, binary}]

    @callback some_function(term) :: :ok | {:error, term}

    @macrocallback macro_name(term) :: Macro.t()

    @optional_callbacks macro_name: 1

    @doc false
    defmacro __using__(_opts), do: :no_op

    @doc """
    Menentukan kapan penggunaan `:ok`. Hal ini diperbolehkan dalam guards
    """
    defguard is_ok(term) when term == :ok

    @impl true
    def init(state), do: {:ok, state}

    # Definisikan fungsi lainnya di sini.
  end
  ```

* <a name="module-pseudo-variable"></a>
  Gunakan *pseudo variable* `__MODULE__` ketika sebuah `module` mengacu pada dirinya sendiri. Hal ini untuk menghindari pembaruan ketika nama `module` berganti.
  <sup>[[tautan](#module-pseudo-variable)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    defstruct [:name]

    def name(%__MODULE__{name: name}), do: name
  end
  ```

* <a name="alias-self-referencing-modules"></a>
  If you want a prettier name for a module self-reference, set up an alias.
  <sup>[[tautan](#alias-self-referencing-modules)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    alias __MODULE__, as: SomeModule

    defstruct [:name]

    def name(%SomeModule{name: name}), do: name
  end
  ```

* <a name="repetitive-module-names"></a>
  Avoid repeating fragments in module names and namespaces.
  This improves overall readability and
  eliminates [ambiguous aliases][Conflicting Aliases].
  <sup>[[tautan](#repetitive-module-names)]</sup>

  ```elixir
  # tidak dianjurkan
  defmodule Todo.Todo do
    ...
  end

  # dianjurkan
  defmodule Todo.Item do
    ...
  end
  ```

### Documentation

Documentation in Elixir (when read either in `iex` with `h` or generated with
[ExDoc]) uses the [Module Attributes] `@moduledoc` and `@doc`.

* <a name="moduledocs"></a>
  Always include a `@moduledoc` attribute in the line right after `defmodule` in
  your module.
  <sup>[[tautan](#moduledocs)]</sup>

  ```elixir
  # tidak dianjurkan

  defmodule AnotherModule do
    use SomeModule

    @moduledoc """
    About the module
    """
    ...
  end

  # dianjurkan

  defmodule AThirdModule do
    @moduledoc """
    About the module
    """

    use SomeModule
    ...
  end
  ```

* <a name="moduledoc-false"></a>
  Use `@moduledoc false` if you do not intend on documenting the module.
  <sup>[[tautan](#moduledoc-false)]</sup>

  ```elixir
  defmodule SomeModule do
    @moduledoc false
    ...
  end
  ```

* <a name="moduledoc-spacing"></a>
  Separate code after the `@moduledoc` with a blank line.
  <sup>[[tautan](#moduledoc-spacing)]</sup>

  ```elixir
  # tidak dianjurkan
  defmodule SomeModule do
    @moduledoc """
    About the module
    """
    use AnotherModule
  end

  # dianjurkan
  defmodule SomeModule do
    @moduledoc """
    About the module
    """

    use AnotherModule
  end
  ```

* <a name="heredocs"></a>
  Use heredocs with markdown for documentation.
  <sup>[[tautan](#heredocs)]</sup>

  ```elixir
  # tidak dianjurkan
  defmodule SomeModule do
    @moduledoc "About the module"
  end

  defmodule SomeModule do
    @moduledoc """
    About the module

    Examples:
    iex> SomeModule.some_function
    :result
    """
  end

  # dianjurkan
  defmodule SomeModule do
    @moduledoc """
    About the module

    ## Examples

        iex> SomeModule.some_function
        :result
    """
  end
  ```

### Typespecs

Typespecs are notation for declaring types and specifications, for
documentation or for the static analysis tool Dialyzer.

Custom types should be defined at the top of the module with the other
directives (see [Modules](#modules)).

* <a name="typedocs"></a>
  Place `@typedoc` and `@type` definitions together, and separate each
  pair with a blank line.
  <sup>[[tautan](#typedocs)]</sup>

  ```elixir
  defmodule SomeModule do
    @moduledoc false

    @typedoc "The name"
    @type name :: atom

    @typedoc "The result"
    @type result :: {:ok, term} | {:error, term}

    ...
  end
  ```

* <a name="union-types"></a>
  If a union type is too long to fit on a single line, put each part of the
  type on a separate line, indented one level past the name of the type.
  <sup>[[tautan](#union-types)]</sup>

  ```elixir
  # tidak dianjurkan
  @type long_union_type ::
          some_type | another_type | some_other_type | one_more_type | a_final_type

  # dianjurkan
  @type long_union_type ::
          some_type
          | another_type
          | some_other_type
          | one_more_type
          | a_final_type
  ```

* <a name="naming-main-types"></a>
  Name the main type for a module `t`, for example: the type specification for a
  struct.
  <sup>[[tautan](#naming-main-types)]</sup>

  ```elixir
  defstruct [:name, params: []]

  @type t :: %__MODULE__{
          name: String.t() | nil,
          params: Keyword.t()
        }
  ```

* <a name="spec-spacing"></a>
  Place specifications right before the function definition,
  after the `@doc`,
  without separating them by a blank line.
  <sup>[[tautan](#spec-spacing)]</sup>

  ```elixir
  @doc """
  Some function description.
  """
  @spec some_function(term) :: result
  def some_function(some_data) do
    {:ok, some_data}
  end
  ```

### Structs

* <a name="nil-struct-field-defaults"></a>
  Use a list of atoms for struct fields that default to `nil`, followed by the
  other keywords.
  <sup>[[tautan](#nil-struct-field-defaults)]</sup>

  ```elixir
  # tidak dianjurkan
  defstruct name: nil, params: nil, active: true

  # dianjurkan
  defstruct [:name, :params, active: true]
  ```

* <a name="struct-def-brackets"></a>
  Omit square brackets when the argument of a `defstruct` is a keyword list.
  <sup>[[tautan](#struct-def-brackets)]</sup>

  ```elixir
  # tidak dianjurkan
  defstruct [params: [], active: true]

  # dianjurkan
  defstruct params: [], active: true

  # required - brackets are not optional, with at least one atom in the list
  defstruct [:name, params: [], active: true]
  ```

* <a name="multiline-structs"></a>
  If a struct definition spans multiple lines, put each element on its own line,
  keeping the elements aligned.
  <sup>[[tautan](#multiline-structs)]</sup>

  ```elixir
  defstruct foo: "test",
            bar: true,
            baz: false,
            qux: false,
            quux: 1
  ```

  If a multiline struct requires brackets, format it as a multiline list:

  ```elixir
  defstruct [
    :name,
    params: [],
    active: true
  ]
  ```

### Exceptions

* <a name="exception-names"></a>
  Make exception names end with a trailing `Error`.
  <sup>[[tautan](#exception-names)]</sup>

  ```elixir
  # tidak dianjurkan
  defmodule BadHTTPCode do
    defexception [:message]
  end

  defmodule BadHTTPCodeException do
    defexception [:message]
  end

  # dianjurkan
  defmodule BadHTTPCodeError do
    defexception [:message]
  end
  ```

* <a name="lowercase-error-messages"></a>
  Use lowercase error messages when raising exceptions, with no trailing
  punctuation.
  <sup>[[tautan](#lowercase-error-messages)]</sup>

  ```elixir
  # tidak dianjurkan
  raise ArgumentError, "This is not valid."

  # dianjurkan
  raise ArgumentError, "this is not valid"
  ```

### Collections

* <a name="keyword-list-syntax"></a>
  Always use the special syntax for keyword lists.
  <sup>[[tautan](#keyword-list-syntax)]</sup>

  ```elixir
  # tidak dianjurkan
  some_value = [{:a, "baz"}, {:b, "qux"}]

  # dianjurkan
  some_value = [a: "baz", b: "qux"]
  ```

* <a name="map-key-atom"></a>
  Use the shorthand key-value syntax for maps when all of the keys are atoms.
  <sup>[[tautan](#map-key-atom)]</sup>

  ```elixir
  # tidak dianjurkan
  %{:a => 1, :b => 2, :c => 0}

  # dianjurkan
  %{a: 1, b: 2, c: 3}
  ```

* <a name="map-key-arrow"></a>
  Use the verbose key-value syntax for maps if any key is not an atom.
  <sup>[[tautan](#map-key-arrow)]</sup>

  ```elixir
  # tidak dianjurkan
  %{"c" => 0, a: 1, b: 2}

  # dianjurkan
  %{:a => 1, :b => 2, "c" => 0}
  ```

### Strings

* <a name="strings-matching-with-concatenator"></a>
  Match strings using the string concatenator rather than binary patterns:
  <sup>[[tautan](#strings-matching-with-concatenator)]</sup>

  ```elixir
  # tidak dianjurkan
  <<"my"::utf8, _rest::bytes>> = "my string"

  # dianjurkan
  "my" <> _rest = "my string"
  ```

### Regular Expressions

_No guidelines for regular expressions have been added yet._

### Metaprogramming

* <a name="avoid-metaprogramming"></a>
  Avoid needless metaprogramming.
  <sup>[[tautan](#avoid-metaprogramming)]</sup>

### Testing

* <a name="testing-assert-order"></a>
  When writing [ExUnit] assertions, put the expression being tested to the left
  of the operator, and the expected result to the right, unless the assertion is
  a pattern match.
  <sup>[[tautan](#testing-assert-order)]</sup>

  ```elixir
  # dianjurkan
  assert actual_function(1) == true

  # tidak dianjurkan
  assert true == actual_function(1)

  # required - the assertion is a pattern match
  assert {:ok, expected} = actual_function(3)
  ```

## Resources

### Alternative Style Guides

* [Aleksei Magusev's Elixir Style Guide](https://github.com/lexmag/elixir-style-guide#readme)
  — An opinionated Elixir style guide stemming from the coding style practiced
  in the Elixir core libraries.
  Developed by [Aleksei Magusev](https://github.com/lexmag) and
  [Andrea Leopardi](https://github.com/whatyouhide), members of Elixir core team.
  While the Elixir project doesn't adhere to any specific style guide,
  this is the closest available guide to its conventions.

* [Credo's Elixir Style Guide](https://github.com/rrrene/elixir-style-guide#readme)
  — Style Guide for the Elixir language, implemented by
  [Credo](http://credo-ci.org) static code analysis tool.

### Tools

Refer to [Awesome Elixir][Code Analysis] for libraries and tools that can help
with code analysis and style linting.

## Getting Involved

### Contributing

It's our hope that this will become a central hub for community discussion on
best practices in Elixir.
Feel free to open tickets or send pull requests with improvements.
Thanks in advance for your help!

Check the [contributing guidelines][Contributing] for more information.

### Spread the Word

A community style guide is meaningless without the community's support. Please
tweet, [star][Stargazers], and let any Elixir programmer know
about [this guide][Elixir Style Guide] so they can contribute.

## Copying

### License

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a
[Creative Commons Attribution 3.0 Unported License][License]

### Attribution

The structure of this guide, bits of example code, and many of the initial
points made in this document were borrowed from the [Ruby community style guide].
A lot of things were applicable to Elixir and allowed us to get _some_ document
out quicker to start the conversation.

Here's the [list of people who have kindly contributed][Contributors] to this
project.

<!-- Links -->
[Chinese Simplified]: https://github.com/geekerzp/elixir_style_guide/blob/master/README-zhCN.md
[Chinese Traditional]: https://github.com/elixirtw/elixir_style_guide/blob/master/README_zhTW.md
[Code Analysis]: https://github.com/h4cc/awesome-elixir#code-analysis
[Code Of Conduct]: https://github.com/elixir-lang/elixir/blob/master/CODE_OF_CONDUCT.md
[Code Formatter]: https://hexdocs.pm/elixir/Code.html#format_string!/2
[Conflicting Aliases]: https://elixirforum.com/t/using-aliases-for-fubar-fubar-named-module/1723
[Contributing]: https://github.com/christopheradams/elixir_style_guide/blob/master/CONTRIBUTING.md
[Contributors]: https://github.com/christopheradams/elixir_style_guide/graphs/contributors
[Elixir Style Guide]: https://github.com/christopheradams/elixir_style_guide
[Elixir]: http://elixir-lang.org
[ExDoc]: https://github.com/elixir-lang/ex_doc
[ExUnit]: https://hexdocs.pm/ex_unit/ExUnit.html
[French]: https://github.com/ronanboiteau/elixir_style_guide/blob/master/README_frFR.md
[Guard Expressions]: https://hexdocs.pm/elixir/guards.html#list-of-allowed-expressions
[Hex]: https://hex.pm/packages
[Indonesian]: https://github.com/AlifArifin/elixir_style_guide/blob/master/README_inID.md
[Japanese]: https://github.com/kenichirow/elixir_style_guide/blob/master/README-jaJP.md
[Korean]: https://github.com/marocchino/elixir_style_guide/blob/new-korean/README-koKR.md
[License]: http://creativecommons.org/licenses/by/3.0/deed.en_US
[Mix format]: https://hexdocs.pm/mix/Mix.Tasks.Format.html
[Module Attributes]: http://elixir-lang.org/getting-started/module-attributes.html#as-annotations
[Naming Conventions]: https://hexdocs.pm/elixir/naming-conventions.html
[Portuguese]: https://github.com/gusaiani/elixir_style_guide/blob/master/README_ptBR.md
[Ruby community style guide]: https://github.com/bbatsov/ruby-style-guide
[Sentence Spacing]: http://en.wikipedia.org/wiki/Sentence_spacing
[Spanish]: https://github.com/iver/elixir_style_guide/blob/spanish/i18n/README_es.md
[Stargazers]: https://github.com/christopheradams/elixir_style_guide/stargazers
