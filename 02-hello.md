# `Object#hello` を作る

## 課題

以下のRubyプログラムと同様の処理を行う `Kernel#hello` 組み込みメソッドを作成する

```ruby
module Kernel
  def hello
    STDOUT.puts "Hello"
  end
end
```

## 達成条件

下記の「やりかた」で追加したテストが通る。

## やりかた

 1. `Kernel`がどこで定義されているか探します。
 2. その付近で、`"hello"`という名前の組み込みメソッドを `rb_f_hello` 関数と紐付けます

    ```c
    rb_define_global_function("hello", rb_f_hello, 0);
    ```
    
 3. 上記のRubyプログラムを参考に `rb_f_hello` を実装します。
 4. `make` が正常に終わるようにします。
 5. ソースコードの `test/ruby/test_io.rb` を開きます。このファイルには `class TestIO` というクラスがひとつだけ定義されています。そこで、このクラスに以下のようなテストを追加します。

    ```ruby
    def test_hello
      assert_in_out_err([], "hello", ["Hello"])
    end
    ```

 6. `make test-all` が正常に終わるようにします。

## ヒント

- `rb_f_hello` はRubyから見ると無引数メソッドですが、Cから見ると「VALUEを受けてVALUEを返す関数」です。つまり、以下のように始まります。

    ```C
    static VALUE rb_f_hello(VALUE self);
    ```
    
- `rb_f_hello` は何を返してもいいですが、まあ`self`をそのまま返すのが楽なんじゃないでしょうか。
