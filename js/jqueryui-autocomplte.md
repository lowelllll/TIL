# JqueryUI Autocomplete

### problem

미세먼지 웹을 개발하는 중 사용자의 지역을 입력할 때 '서'만 쳐도 '서울'이 나오고 '서울'을 치면 '서울 관악구','서울 강남구'... 이렇게 나타나도록 만들고 싶었다.

jquery의 `change`로 비슷하게 기능을 구현할 수 있었는데 `change` 함수는 사용자가 입력을 하고  입력 창에서 focus가 해제되어야 값이 바뀐 것을 인식한다.

`change`는 내가 생각한 기능이 아니여서 패스..하고 사람들은 이런 것을 어떻게 구현할까 하고 찾아보다가 <b>검색어 자동 완성 기능</b>을 알게 되었고 이것이 내가 만들고 싶은 기능이라는 것을  알게되었다.

### solution

검색어 자동 완성 기능은 JqueryUI로 쉽게 구현할 수 있었다.

JqueryUI을 사용하기 위해서는 다운로드해야 했지만 나는 다운로드는 별로 안좋아해서(귀찮..) 그냥 google cdn 사용했다.

[JqueryUI google cdn](https://developers.google.com/speed/libraries/#jquery-ui)

여기서 snippet을 따라 link와 src 걸어주면 된다.

```html
<!--Jquery-->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.4/jquery.min.js"></script>
<!--JqueryUI-->
<link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/themes/smoothness/jquery-ui.css">
<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/jquery-ui.min.js"></script> 
..
<input type="text" id="text">
<script>
var autocomplete_text = ["서울","경기","부산"]; // 자동완성 될 텍스트
    $('#text').autocomplete({ // jqueryui method 
      source:autocomplete_text 
    });
</script>
```



이렇게 코드를 작성하면 `autocomplete_text`를 반영해서 '서'만 입력해도 '서울'이라고 밑에 자동완성이 뜬다.

## refer

[http://hellogk.tistory.com/74](http://hellogk.tistory.com/74)

