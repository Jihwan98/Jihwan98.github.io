---
title: "[Django] SerializerMethodField로 원하는 JSON 보내주기"
author: JIHWAN PARK
categories: [Backend, Django]
tag: [Backend, Django]
math: true
date: 2023-07-02 18:55:34 +0900
last_modified_at: 2023-07-02 18:55:34 +0900
---

> Django에서 SerializerMethodField로 원하는 JSON을 보내주기
> {: .prompt-info}

만약

- 모델에 없는 Field인데 JSON에 추가에서 보내주고 싶거나
- 모델에 있는 값을 변형해서 JSON으로 보내고 싶거나
- 외래키로 연결되어 있는 값을 JSON으로 보내고 싶거나 (나의 경우는 여기 해당했었다)

하는 경우에는 [SerializerMethodField](https://www.django-rest-framework.org/api-guide/fields/#serializermethodfield)를 사용하면 된다.

나의 경우엔 Question과 User가 외래키로 연결되어있었고, Question을 조회할 때 User의 first_name을 전달해주고 싶었다.

```python
# serializers.py

class QuestionSerializer(ModelSerializer):
    ...

    user_name = serializers.SerializerMethodField(method_name='get_user_name')

    ...

    def get_user_name(self, obj):
        user = obj.userid
        return user.first_name

    ...
```
