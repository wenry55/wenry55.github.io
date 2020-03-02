---
layout: default
title: Development Guide
categories: [project]
tags: [guide, modal, ]
---


##### 수정시 부모창에서 Modal로 데이터 넘기기

```js
//  JSON 데이터를 복제하는 방식을 사용하여, 부모창의 데이터와의 종속관계를 끊고 사용하도록 합니다.
modal_data = JSON.Parse(JSON.stringify(selected_data))
```

##### Modal 화면 모드 (New, Edit, Read) 에 따른 Flag 사용

* New 일 경우는 modal_data = null 을 할당하여 New 인지 알 수 있습니다. 
 Edit 모드에서 수정하면 안되는 필드에 대한 Disable 처리
* isReadOnly : Read(상세보기) 인 경우 필드의 Disable 처리를 위해 사용

##### 여러개의 레코드를 삭제할 경우 사용되는 http verb => delete 사용

##### HTTP Verb의 사용

Q) Delete 를 del_yn (Flag처리) 로 하는 경우 실제 수행 쿼리는 Update 임, 이 때 http delete 호출? Or put 호출??

A) 위의 경우는 put을 사용하면 됩니다. 개념상 서버에서 실제로 레코드가 삭제되는 경우 delete verb를 사용합니다. 실제 서버에서 update가 일어나는 경우는 put을 사용하면 됩니다.

Q) Delete Many인 경우 Body 객체가 넘어가지 않는데, object id를 url 파라미터로 “,” 로 길게 보내는 게 맞는지 ? put 을 사용해서 body로 넘길지?
확장해서.. Select의 조건이 여러 개인 경우는 어떻게???

A) http delete verb는 body를 허용하지 않습니다. 사실 multiple delete는 batch 성격으로 보고, http post 를 이용하여 body를 사용하는 게 좋습니다. 즉, batch/transaction 성격의 `request`는 http post를 이용하세요. Select의 조건이 여러개일 경우는 http get에 아래와 같이 url parameter를 이용하는게 좋습니다. 

```
https://site.com/page?from=1&to=10&age=10
```

Q) put을 사용한다고 하면, Update시 사용하는 put 과의 url 구분은 어떻게???
post를 사용하여 request 함수를 만드는 것이 좋습니다. 


##### Unique한 사용자 입력 필드의 입력값 Validation 

사용자 ID와 같이 사용자가 직접 입력하는 필으의 경우, 키 입력시 _.debounce 를 이용해서 체크하는게 더 좋을것 같습니다. 서버에 리소스가 부족하지 않고, 부하를 일으킬만한 요소가 아직은 없어보이니, 키입력시 실시간으로 자동체크 하는것이 좋을것 같습니다. `_.debounce()` 를 활용하도록 합니다.



Ø  Code 성격의 데이터를 Name으로 매핑하는 것은 Vue 단에서 데이터 조회 시 수행

ü  vue에서 mounted에 코드성 데이터조회 à 메인 데이터 조회 순으로 조회

ü  코드성 데이터는 in 조건을 사용해서 다수의 코드를 한번에 조회함 (AdminPlantMgmt.vue 파일의 getCodeData() 참고)
à 데이터를 받아오면 vue에서 각각의 코드 배열로 분배 (ret.items.filter 사용)
à DB에 code_id, code_name을 Control에서 요구하는 Key 값으로 변경(ex- code_id à value, code_name à text)

ü  메인데이터 조회 후 map을 사용해서 각각의 코드 배열의 코드Name 정보 추가 (AdminPlantMgmt.vue 파일의 getItemList() 참고)

ü  화면에는 코드성 데이터는 보이지 않게 하고, 코드Name이 보이도록 Field 조정

 