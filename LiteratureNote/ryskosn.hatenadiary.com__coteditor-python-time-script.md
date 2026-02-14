---
type: literature
source_url: https://ryskosn.hatenadiary.com/entry/20100920/1284975691
source_title: CotEditor で現在時刻を挿入する Python スクリプトを作った
created_at: 2026-02-14
updated_at: 2010-09-20
tags:
  - coteditor
  - mac
  - script
  - python
  - datetime
source_type: web
domain: ryskosn.hatenadiary.com
---

이 글은 Mac용 텍스트 편집기인 CotEditor에서 현재 시간을 삽입하는 Python 스크립트를 직접 만들어 활용하는 방법을 소개한다. 윈도우 환경에서 EmEditor의 F5 기능으로 작업 로그를 기록했던 저자가 CotEditor에서 유사한 기능을 구현하고자 시도한 경험을 공유한다.

**Python으로 현재 시간 얻기:**
`datetime` 모듈을 사용하여 `datetime.datetime.today()`로 현재 시간을 얻고, `strftime()` 메서드를 통해 원하는 형식으로 포맷한다. 저자는 `%y-%m-%d %a %X` 형식(년-월-일 요일 시:분:초)을 선택했다.

**CotEditor 스크립트 설정:**
스크립트 파일의 첫 줄에 Shebang(`#!/usr/bin/python`)을 명시하고, CotEditor의 스크립트 기능을 활용하기 위해 `%%%{CotEditorXOutput=ReplaceSelection}%%%` 주석을 추가하여 스크립트 실행 결과로 현재 선택된 텍스트를 대체하도록 설정한다.

**스크립트 파일 배치 및 실행 권한:**
작성한 Python 스크립트 파일을 CotEditor의 스크립트 디렉토리에 배치하고, `chmod` 명령을 통해 실행 권한을 부여한다.

저자는 이 과정에서 Python 코드를 단 3줄만 작성했음에도 불구하고 상당한 시간이 소요되었음을 언급하며, 초반에는 이러한 시행착오가 일반적임을 시사한다. 궁극적으로는 EmEditor와 유사한 방식으로 작업 로그를 기록할 수 있는 환경을 CotEditor에서 구축하는 데 성공했다.
