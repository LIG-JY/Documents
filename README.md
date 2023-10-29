# Practice

## 커밋 메시지 규칙

다음의 규칙에 따라 커밋 메시지를 작성해주세요. 이 규칙은 프로젝트의 일관성을 유지하고, 변동사항을 쉽게 파악하기 위한 목적을 갖고 있습니다.

| Tag Name           | Description                                                           |
|--------------------|-----------------------------------------------------------------------|
| `Feat`             | 새로운 기능을 추가                                                        
| `Fix`              | 버그 수정                                                               
| `Design`           | CSS 등 사용자 UI 디자인 변경                                          
| `!BREAKING CHANGE` | 커다란 API 변경의 경우                                               
| `!HOTFIX`          | 급하게 치명적인 버그를 고쳐야하는 경우                                    
| `Style`            | 코드 포맷 변경, 세미 콜론 누락, 코드 수정이 없는 경우                       
| `Refactor`         | 프로덕션 코드 리팩토링                                                
| `Comment`          | 필요한 주석 추가 및 변경                                              
| `Docs`             | 문서 수정                                                             
| `Test`             | 테스트 코드, 리펙토링 테스트 코드 추가, Production Code 변경 없음     
| `Chore`            | 빌드 업무 수정, 패키지 매니저 수정, 패키지 관리자 구성 등 업데이트    
| `Rename`           | 파일 혹은 폴더명을 수정하거나 옮기는 작업만인 경우                    
| `Remove`           | 파일을 삭제하는 작업만 수행한 경우

### Example)
```
git commit -m "Docs: Add commit message guidelines to README"
```
