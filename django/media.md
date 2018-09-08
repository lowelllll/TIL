

# Media Files

> 유저가 업로드한 모든 정적인 파일 ( image,etc)

## Media file 서빙하기

`project/settings.py` 설정

#### MEDIA_URL

> 현재 요청이 media 파일 요청인지 구분하는 url

STATIC_URL과 유사.

#### MEDIA_ROOT

> 업로드 된 파일을 저장할 디렉토리 경로

STATICFILES_DIR과 유사.



뷰는 `HttpRequest.FILES`를 통해 파일을 전달 받고, 적절히  검증을 거친 다음 `MEDIA_ROOT` 하단에 저장함.

### Media

#### FileField / ImageField

파일 저장을 지원하는 모델 필드.

파일은 `settings.MEDIA_ROOT` 경로의 <b>File System</b>에 저장하며

DB 필드에는 `settings.MEDIA_ROOT` 내 저장된 하위 경로를 저장. (문자열)





 