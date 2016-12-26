# gollum과 pow 연동 시 참고

[refer](http://serverfault.com/questions/184674/how-to-run-gollum-using-mod-rails-and-apache-in-a-shared-hosting-environment-e/223813#223813)
[gollum_git_hub](https://github.com/gollum/gollum)
```
$ [sudo] gem install gollum
```

여기에서 의존성 문제나 기타 문제 때문에 오류가 발생
[link](https://github.com/gollum/gollum/wiki/Installation)

위 링크를 참고해서 해결

pow에서 gollum을 실행하기 위해서 config.ru 파일이 git repo에 필요하다.
```
require "gollum/app"

Precious::App.set(:gollum_path, File.dirname(__FILE__))
Precious::App.set(:wiki_options, {})
run Precious::App
```

그리고 pow를 설치한 뒤 링크해주면 된다.
```
curl get.pow.cx | sh

cd ~/.pow
ln -s /your_repo/ your_name.wiki
```

이제 your_name.wiki.dev로 접속 할 수 있다.
