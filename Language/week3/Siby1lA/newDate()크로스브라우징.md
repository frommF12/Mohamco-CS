# 이슈

DND에서 편의점 서비스를 만들면서 생긴 이슈이다.
나는 캘린더 즉 사용자의 출근부 작성 및 확인이 필요한 기능을 담당했었고 그 기능도 잘 작동했었다. 모바일 웹이다 보니 모바일 사파리에서 들어갔는데 데이터들이 렌더링 되지 않았던 것이다!! 그래서 모바일 크롬으로 갔는데 거기서는 또 잘 보였다. 즉 크롬에서는 데이터들이 잘 보이는데 사파리에서는 안 보여서 발견한 이슈라고 보면 된다.

처음에는 무슨 렌더링 문제나 API 호출한 값이 state에 잘 안 담기나 했는데, 하나씩 디버깅하면서 찾은 결과 `new Date()` 때문에 발생한 이슈였다.

# 원인

## 기존 코드

```
if (!workDay || new Date(clickDay) > new Date(toDay)) {
	// 근무 안된 날짜 클릭시, 미래 클릭시
	return (<... />);
}
```

기존 코드에서 위와 같이 new Date()를 활용해서 날짜들을 계산하는 로직이 많이 있었다.
![](https://velog.velcdn.com/images/psb7391/post/535c9627-0a95-42e8-9e68-9b4fe1468eb8/image.png)
분명히 바텀 시트에서 출근한 유저들의 리스트가 떠야 하는데 사파리에서는 뜨지 않았다.

그래서 위에서 서술한 거와 같이 유저 리스트가 안 보여서 렌더링 문제인 줄 알았으나 저 new Date()의 조건문을 빼고 렌더링 하니 그때는 잘 떴었다. 그래서 아차 사파리에는 css도 제대로 적용 안되는 경우가 있다던데 설마 new Date()도 그런 경우가 있나 해서 찾아보았다. 그 결과 나와 같은 이슈가 진짜 있었다. [블로그 글](https://string.tistory.com/32) 여기서 글을 보고 즉시 해결하는 과정에 들어갔다.

## 해결한 코드

이 이슈를 해결하기 위해서는 아래 2가지 방법이 있다고 한다.
나는 2번째 방법으로 해결하였다.

1. moment.js 라이브러리 사용하기
2. 2020, 09, 01, 00:00:00 형식으로 new Date()에 날짜 넣기

> new Date(2023, 1, 23, 0, 0, 0) // 내 코드

```
export function crossDate(clickDay: string, toDay: string) {
	const [currentYear, currentMonth, currentDay] = clickDay.split('.').map(Number);
	const [toDayYear, toDayMonth, toDayDay] = toDay.split('.').map(Number);
	const clickDayData = new Date(currentYear, currentMonth, currentDay, 0, 0, 0);
	const toDayData = new Date(toDayYear, toDayMonth, toDayDay, 0, 0, 0);
	return { clickDayData, toDayData };
}
```

크로스 브라우징 이슈를 해결하기 위해 new Date()를 다른 브라우저에서도 호환할 수 있게 하였고 이 서비스에서는 new Date()로 형 변환을 자주 써가지고 유틸 함수로 만들었다.

```
if (!workDay || clickDayData > toDayData) {
	// 근무 안된 날짜 클릭시, 미래 클릭시
	return (<... />);
}
```

이는 위 유틸 함수에서 가져온 값들을 비교하는 조건문이다.

![](https://velog.velcdn.com/images/psb7391/post/67940a46-3238-4468-b381-f432e9a7e412/image.png)

이로써 사파리에서도 유저들의 출근 리스트를 볼 수 있게 되었다.

# 느낀점

항상 크롬 브라우저에서 테스트를 했는데 이는 좋지 않은 것을 느꼈다.
크롬이 점유율이 높다고 해도 다른 브라우저 쓰는 사람들도 많기 때문이다. 특히 아이폰 같은 경우는 사파리가 기본 브라우저이고 아이폰 유저가 엄청 많기 때문에 신경 써야 하는 부분이란 것을, 중요하단 것을 몸소 느꼈다.
앞으로 여러 브라우저에서 테스팅 하는 습관을 들이도록 노력해야겠다.

그리고 막상 문제 원인을 알고 나니 해결하기 쉬웠지만, 프로젝트가 여기서 더 진행되었거나 엄청 큰 프로젝트였더라면 엄청 큰일이었을 것이다. 특히 내가 사파리를 사용하지 않았더라면 못 찾았을 문제여서 큰 반성을 했다. 항상 개발할 기능들이 다 구현이 되었다고 생각해도 방심하지 않고, 계속해서 리팩토링을 하는 과정을 거치고 테스트도 여러 환경에서 해봐야 하는 것을 깨달았다. 이래서 스토리북과 같은 TDD가 있나 싶었다.
