---
layout: default 
title: Vue Modal Programming
categories: [project, dev, guide]
tags: [vue, modal, guide]
---

### FAQ & Issues

##### 수정시 부모창에서 Modal로 데이터 넘기기

```js
//  JSON 데이터를 복제하는 방식을 사용하여, 부모창의 데이터와의 종속관계를 끊고 사용하도록 합니다.
modal_data = JSON.Parse(JSON.stringify(selected_data))
```

##### Modal 화면 모드 (New, Edit, Read) 에 따른 Flag 사용

`modal_mode`를 설정하는 것이 좋겠습니다.
```
modal_mode = 'new' // 'edit', 'readonly'

// new : 신규 작성
// edit : 수정
// readonly : 조회(상세보기)
```

readonly : Read(상세보기) 인 경우 필드의 Disable 처리를 위해 사용

##### 여러개의 레코드를 삭제할 경우 사용되는 http verb => delete 사용

##### HTTP Verb의 사용

>Q) Delete 를 del_yn (Flag처리) 로 하는 경우 실제 수행 쿼리는 Update 임, 이 때 http delete 호출? Or put 호출??

A) 위의 경우는 put을 사용하면 됩니다. 개념상 서버에서 실제로 레코드가 삭제되는 경우 delete verb를 사용합니다. 실제 서버에서 update가 일어나는 경우는 put을 사용하면 됩니다.

>Q) Delete Many인 경우 Body 객체가 넘어가지 않는데, object id를 url 파라미터로 “,” 로 길게 보내는 게 맞는지 ? put 을 사용해서 body로 넘길지?
확장해서.. Select의 조건이 여러 개인 경우는 어떻게???

A) http delete verb는 body를 허용하지 않습니다. delete verb와 comma separated list를 사용하는 것이 일반적입니다. 아래 mozilla 프로젝트에서 정의한 API를 참조하면 도움이 될겁니다.   
Select의 경우도 동일(comma separated list)합니다. 서로다른 항목을 filtering 할 경우는 각 항목마다 param이 추가됩니다.

##### 모질라 프로젝트에서의 DELETE API 

[Mozilla Storage API](https://moz-services-docs.readthedocs.io/en/latest/storage/apis-1.5.html#api-instructions)

위에 정의된 API중 DELETE를 살펴보면, 

* 컬렉션 전체를 삭제하고자 할 때.
>DELETE https://\<endpoint-url>/storage/\<collection>  
> 
>Deletes an entire collection. 
>
>After executing this request, the collection will not appear in the output of GET /info/collections and calls to GET /storage/<collection> will return an empty list.

* <span style='color:red'>여러개의 레코드</span>를 삭제하고자 할 경우.
>DELETE https://\<endpoint-url>/storage/\<collection>?ids=\<ids>
>
>Deletes multiple BSOs from a collection with a single request.
>
>This request takes a parameter to select which items to delete:
>
>ids: deletes BSOs from the collection whose ids that are in the provided `comma-separated list`. A maximum of 100 ids may be provided.
>The collection itself will still exist on the server after executing this request. Even if all the BSOs in the collection are deleted, it will >receive an updated last-modified time, appear in the output of GET /info/collections, and be readable via GET /storage/<collection>
>
>Successful responses will have a JSON object body with field “modified” giving the new last-modified time for the collection.
>
>Potential HTTP error responses include:
>
>400 Bad Request: too many ids were included in the query parameter. => 한번에 삭제할 수 있는 아이템의 제한은 서버사이드에서 한번더 확인됩니다.

* 특정 레코드만 삭제할 경우
>DELETE https://\<endpoint-url>/storage/\<collection>/\<id>
>
>Deletes the BSO at the given location.

Q) put을 사용한다고 하면, Update시 사용하는 put 과의 url 구분은 어떻게???
위에서 설명한대로, delete 와 comma separated list를 이용합니다.


##### Unique한 사용자 입력 필드의 입력값 Validation 

사용자 ID와 같이 사용자가 직접 입력하는 필으의 경우, 키 입력시 _.debounce 를 이용해서 체크하는게 더 좋을것 같습니다. 서버에 리소스가 부족하지 않고, 부하를 일으킬만한 요소가 아직은 없어보이니, 키입력시 실시간으로 자동체크 하는것이 좋을것 같습니다. `_.debounce()` 를 활용하도록 합니다.

 

##### Code 성격의 데이터를 Name으로 매핑하는 것은 Vue 단에서 데이터 조회 시 수행

Suggested by : shape

* vue에서 mounted에 코드성 데이터조회 -> 메인 데이터 조회 순으로 조회
* 코드성 데이터는 in 조건을 사용해서 다수의 코드를 한번에 조회함 (AdminPlantMgmt.vue 파일의 getCodeData() 참고)

  * 데이터를 받아오면 vue에서 각각의 코드 배열로 분배 (ret.items.filter 사용)
  * DB에 code_id, code_name을 Control에서 요구하는 Key 값으로 변경(ex- code_id à value, code_name à text)

* 메인데이터 조회 후 map을 사용해서 각각의 코드 배열의 코드Name 정보 추가 (AdminPlantMgmt.vue 파일의 getItemList() 참고)
* 화면에는 코드성 데이터는 보이지 않게 하고, 코드Name이 보이도록 Field 조정

