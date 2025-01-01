
옵시디언에서 글 작성 → `blog-post` 레포지토리로 `push` → 

### `blog-post` 의 yml (github actions) 

- 토큰을 만들고
- permissions 를 추가하고 
- `cp ./*.md jjaneyxx.github.io/_posts/  # 루트 디렉토리의 모든 .md 파일을 복사` 를 추가했다

```bash
name: Deploy Blog Post to jjaneyxx.github.io

permissions:
  contents: write  # 레포지토리의 내용을 쓸 수 있도록 허용

on:
  push:
    branches:
      - main  # 'main' 브랜치에 푸시될 때마다 실행

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout blog-post repository
      uses: actions/checkout@v2

    - name: Checkout jjaneyxx.github.io repository
      uses: actions/checkout@v2
      with:
        repository: jjaneyxx/jjaneyxx.github.io  # 'jjaneyxx.github.io' 레포지토리
        token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
        path: jjaneyxx.github.io  # 'jjaneyxx.github.io' 레포지토리는 별도의 폴더로 클론

    - name: Copy new blog post to jjaneyxx.github.io repository
      run: |
         cp ./*.md jjaneyxx.github.io/_posts/  # 루트 디렉토리의 모든 .md 파일을 복사

    - name: Commit and push changes to jjaneyxx.github.io repository
      run: |
        cd jjaneyxx.github.io
        git config user.name "Your Name"
        git config user.email "your.email@example.com"
        git add .
        git commit -m "Add new blog post"
        git push origin master
```