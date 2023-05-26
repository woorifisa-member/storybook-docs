# Storybook-docs


![](/image/storybook.png)

## 스토리북이란?

>  `Storybook`은 UI 컴포넌트 개발을 위한 도구로 프론트엔드 개발자들이 개발한 컴포넌트들을 테스트하고, 문서화하고, 개발자 간에 공유하기 쉽게 만들어줍니다.

* 일반적으로 프론트엔드 개발에서는 컴포넌트를 개발할 때, 해당 컴포너트가 다른 컴포넌트와 어떻게 동작하며, 어떤 속성을 가지고 있는지 등을 개발자들끼리 소통하며 개발합니다.

* 이 때, Storybook을 사용하면 개발한 컴포넌트를 직접 실행해 보지 않고도, 컴포넌트의 동작과 속성 등을 미리 확인할 수 있습니다.

* 그리고 Storybook은 컴포넌트들을 **시각적**으로 정리하고, 컴포넌트마다 **문서화**를 할 수 있어, 다른 개발자들과 공유할 때도 편리합니다.

* 이를 통해, 다른 개발자들이 컴포넌트를 쉽게 이해하고 재사용할 수 있도록 도와줍니다.

스토리북에 대한 장점을 정리하자면 아래와 같습니다.

> 1. 시간단축: 개발 시간을 단축시킨다.
> 2. 테스트: 테스트를 통해 오류를 줄일 수 있다.
> 3. 문서화: 컴포넌트를 시각적으로 정리하고 문서화할 수 있다.
> 4. 재사용: 다른 개발자들이 컴포턴트를 쉽게 이해하고 다른 기능 개발에도 활용할 수 있어 재사용할 수 있다.

스토리북이 어떤 위치에 있는지 아래 그림과 같습니다.

![](/image/storybook-design-system.png)

1. PM이 Jira에서 프로젝트 기획서를 작성한다.
2. PM, Designer가 Figma, AdobeXD를 통해 UI/UX를 설계 및 디자인한다.
3. Developer가 VScode / Intellj를 통해 개발을 한다.
4. Frontend Developer가 본인의 코드를 테스트 하기 위해 **Storybook**를 통해 확인이 가능하다. 뿐만 아니라 다른 컴포넌트를 개발할 때도 재사용화할 수 있다. 


## 회사에서 Storybook 사용 안하는 이유

> 회사가 작은경우

* 규모가 작아 문서로 공유할 필요가 적다.

* 다른 컴포넌트 라이브러리(daisy,ui,mui)를 사용하게 된다.

* 컴포넌트를 아무리 만들어도, 실제 화면에서 컴포넌트가 차지하는 비중이 적다.

> 회사가 큰 경우

* 회사가 크면 여러 개의 개발팀이 각자의 서비스를 담당하고 있는 경우가 많다.

* 회사 규모가 커지면 매 팀마다 컴포넌트를 각자 만드는 것은 부담스러운 일입니다. 

* 문서화를 한다고 해도, 정해진 레이아웃에 맞춰야 하니 아무리 커스텀이 가능하다고 해도 한계가 있다.

## 장점1. MockApi로 프론트 개발 마이웨이로 진행

> install: npx storybook@latest init

> ❌ npm run dev

> ⭕️ npm run storybook

포스팅을 보여주는 페이지라면 PostApi를 이렇게 만들어 놓을 수 있겠죠.

```Typescript
interface PostApi {
  findAll(): Promise<{title: string; content: string;}[]>
}

class MockPostApi implements PostApi {
  private posts: {title: string; content: string}[] = [
	  { 
			title: '스토리북이란?',
			content: '매우 좋음'
	  },
	  {
			title: 'Flutter ContraintBox에 대해 알아보자',
			content: '다음에 알아보자'
	  }
	]

  override async findAll(): Promise<{
     title: string
		 content: string
  }[]>  {

	 return this.posts
  }
}
```

페이지는 아래처럼 구현하면 됩니다.

```Typescript
<script lang="ts">
	export let fetchPosts: () => Promise<{title: string; content: string;}[]>
</script>
```

실제 postsApi의 리턴타입이 예상과 달라지면, Dto 변환을 만들어주면 됩니다.

```Typescript
class RealPostApi implements PostApi {
  override async findAll(): Promise<{
     title: string
		 content: string
  }[]>  {
	 const result = await fetch(new URL('/posts', 'https://api.{realApiHost}'))
									.then(res => res.json())
	 return result.map(this.convertDto)
  }

	private convertDto(post: {name: string, decription: string}) {
		return {
			title: name,
			content: description
  }
}
```

이렇게 만들어 놓으면 Storybook 사용과는 별개로 **테스트도 용이**하고, 나중에 프로젝트 할때 백엔드 개발자에게 api 개발 언제 완료되는지 눈치 주지 않아도 됩니다.

## 장점2.  디자이너, 기획자와 소통하기 편함

1. 화면 갱신 주기가 빠르다.


2. 각 화면의 단계를 한눈에 살펴 볼 수 있다.

[1] 서비스에 로그인/회원가입한다.

[2] 메인페이지에 들어간다.

[3] 상품을 구매한다.

[4] 구매한 상품을 마이페이지에서 확인 가능하다.

## Reference

* [왜 스토리북인가](https://storybook.js.org/docs/vue/get-started/why-storybook)

* [Storybook 어디까지 사용해봤니?](https://cozyblog.io/@moon/posts/storybook)


* [[번역] 왜 2022년에는 스토리북일까요?](https://velog.io/@cookie004/why-storybook-in-2022)