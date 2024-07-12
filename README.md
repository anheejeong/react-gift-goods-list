## ✏️ 요구사항

### Week 1

📝 1단계

- 메인 페이지 - Theme 카테고리 섹션
    - [ ] **`/api/v1/themes`**
- 메인 페이지 - 실시간 급상승 선물랭킹 섹션
    - [ ] **`/api/v1/ranking/products`**
    - [ ] 필터링 구현
- Theme 페이지 - header
    - [ ] url의 pathParams와 **`/api/v1/themes`**
    - [ ] **`themeKey`**가 잘못 된 경우 메인 페이지로 연결
- Theme 페이지 - 상품 목록 섹션
    - [ ] **`/api/v1/themes/{themeKey}/products`**
    - [ ] API 요청 시 한번에 20개의 상품 목록이 내려오도록

📝 2단계

- API **Loading** 상태 UI 구현 및 연결
- **데이터가 없는 경우** UI 구현 및 연결
- **HTTP Status**에 따라 Error 처리

📝 3단계
- 스크롤을 내리면 추가로 데이터를 요청
- 1단계에서 구현한 API를 react-query를 사용해서 구현

📝 4단계

1. CORS 에러는 무엇이고 언제 발생하는지 설명해주세요. 이를 해결할 수 있는 방법에 대해서도 설명해주세요.

CORS (Cross-Origin Resource Sharing) 에러는 브라우저 보안 정책에 의해 발생하는 에러이다. 브라우저는 보안을 위해 웹 페이지가 로드된 도메인과 다른 도메인에서 리소스를 요청할 때 이를 제한한다. 이를 SOP (Same-Origin Policy)라고 한다. CORS는 이러한 제한을 완화할 수 있는 메커니즘이다.

**CORS 에러 발생 시점**

- 다른 도메인에서 리소스를 요청할 때 (ex. https://example.com에서 로드된 페이지가 https://api.anotherdomain.com에 요청)
- 다른 포트에서 리소스를 요청할 때 (ex. https://example.com:3000에서 로드된 페이지가 https://example.com:4000에 요청)
- 다른 프로토콜에서 리소스를 요청할 때 (ex. http://example.com에서 로드된 페이지가 https://example.com에 요청)

**CORS 에러의 해결 방법**

[ ] 서버 측 해결 방법
- 적절한 CORS 헤더 설정: 서버가 특정 도메인이나 모든 도메인으로부터의 요청을 허용하도록 응답 헤더에 CORS 설정을 추가
```tsx
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: https://example.com
```
- 추가적인 CORS 헤더 설정: 필요한 경우 다른 CORS 관련 헤더도 설정
```tsx
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
```
- 프리플라이트 요청 처리: 브라우저는 특정 조건을 만족하는 요청에 대해 OPTIONS 메서드로 프리플라이트 요청을 보내고 서버는 이에 대해 적절하게 응답해야 한다.
```tsx
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

[ ] 클라이언트 측 해결 방법
- 프록시 서버 사용: 클라이언트와 서버 사이에 프록시 서버를 두어 모든 요청이 같은 도메인에서 발생하는 것처럼 만들 수 있다.
- JSONP 사용: JSONP는 주로 GET 요청에서 사용되는 오래된 기술로, 서버 응답을 자바스크립트 콜백 함수로 감싸서 CORS 문제를 우회할 수 있게 한다. 그러나 이는 보안상 문제가 될 수 있어 현대적인 애플리케이션에서는 권장되지 않는다.

2. 비동기 처리 방법인 callback, promise, async await에 대해 각각 장단점과 함께 설명해주세요.

**callback**
콜백 함수는 다른 함수에 인수로 전달되어 특정 작업이 완료된 후 호출되는 함수이다. 자바스크립트에서 가장 기본적인 비동기 처리 방식이다.
```tsx
function fetchData(callback) {
    setTimeout(() => {
        const data = { message: "Hello, World!" };
        callback(data);
    }, 1000);
}

fetchData((result) => {
    console.log(result.message);
});
```
장점
- 구현이 단순하며, 기본적인 비동기 작업을 수행하는 데 적합
- 광범위한 지원: 자바스크립트의 오래된 방식으로, 거의 모든 환경에서 사용
단점
- 콜백 지옥: 중첩된 콜백 함수가 많아지면 코드의 가독성이 떨어지고 유지보수가 어려워지는 문제가 발생
- 에러 처리: 콜백 기반의 에러 처리는 복잡할 수 있으며, 여러 콜백에서 에러를 처리하는 로직이 중복될 수 있다.

**Promise**
프로미스는 비동기 작업의 완료 또는 실패를 나타내는 객체이다.. 프로미스는 비동기 작업이 성공하면 then 메서드를, 실패하면 catch 메서드를 호출한다.
```tsx
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const data = { message: "Hello, World!" };
            resolve(data);
        }, 1000);
    });
}

fetchData()
    .then(result => {
        console.log(result.message);
    })
    .catch(error => {
        console.error(error);
    });
```
장점
- 가독성 향상: 콜백 지옥을 피할 수 있으며, 코드가 더 직관적이고 읽기 쉬워진다.
- 체이닝: then과 catch를 통해 여러 비동기 작업을 순차적으로 처리할 수 있다.
- 에러 처리: 프로미스 체인에서 발생한 모든 에러를 하나의 catch 블록에서 처리할 수 있다.
단점
- 상대적 복잡성: 콜백보다 약간 복잡하며, 특히 처음 배우는 사람들에게는 익숙해지는 데 시간이 걸릴 수 있다.
- 중첩된 체인: 여러 비동기 작업을 처리하는 경우 여전히 복잡해질 수 있다.

**Async/Await**
async와 await 키워드를 사용하면 프로미스를 더 쉽게 처리할 수 있다. async 함수는 항상 프로미스를 반환하며, await 키워드는 프로미스가 해결될 때까지 기다린다.
```tsx
async function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const data = { message: "Hello, World!" };
            resolve(data);
        }, 1000);
    });
}

async function main() {
    try {
        const result = await fetchData();
        console.log(result.message);
    } catch (error) {
        console.error(error);
    }
}

main();
```
장점
- 가독성 향상: 동기식 코드처럼 보이기 때문에 코드가 훨씬 더 읽기 쉽고 직관적이다.
- 에러 처리: try/catch 블록을 사용하여 동기식 코드처럼 에러를 처리할 수 있다.
- 코드 구조: 복잡한 비동기 로직을 단순하게 만들 수 있으며, 코드 구조가 더 명확해진다.
단점
- 브라우저 호환성: 오래된 브라우저에서는 지원되지 않을 수 있으므로, 트랜스파일링이 필요할 수 있다.
- 비동기 루프: forEach와 같은 배열 메서드에서 await를 사용할 수 없으며, for...of 루프를 사용해야 한다.

3. react query의 주요 특징에 대해 설명하고, queryKey는 어떤 역할을 하는지 설명해주세요.

**React Query의 주요 특징**

- 데이터 페칭 및 캐싱: React Query는 서버로부터 데이터를 쉽게 페칭하고, 이 데이터를 클라이언트에 캐싱할 수 있도록 한다. 이를 통해 동일한 데이터 요청 시 서버 요청을 최소화하고, 빠른 응답을 제공한다.
- 자동 리페칭: 백그라운드에서 데이터를 자동으로 리페칭하여 최신 상태를 유지한다. 예를 들어, 탭이 다시 활성화되거나, 네트워크가 재연결되면 데이터가 자동으로 리페칭된다.
- 동기화: 여러 컴포넌트가 동일한 데이터를 사용할 때, 데이터가 동기화되어 모든 컴포넌트가 최신 데이터를 가질 수 있도록 한다.
- 캐시 무효화: 특정 이벤트(예: 데이터 수정)가 발생하면 캐시된 데이터를 무효화하고 최신 데이터를 다시 페칭한다.
- 서버 상태 관리: 클라이언트 측 상태와 서버 측 상태를 분리하여 관리할 수 있다. 이를 통해 서버 측 상태를 쉽게 동기화하고, 클라이언트 측 상태와 독립적으로 관리할 수 있다.
- 성능 최적화: 데이터를 사용할 때 필요한 최소한의 리소스를 사용하여 성능을 최적화한다. 필요하지 않은 경우에는 데이터를 페칭하지 않도록 한다.
- 플러그인 및 확장성: 다양한 플러그인과 중간자를 통해 기능을 확장할 수 있다. 이를 통해 필요에 따라 기능을 추가하거나 맞춤화할 수 있다.
- 개발자 경험: 개발 중에도 데이터를 쉽게 검사하고, 디버그할 수 있는 도구를 제공한다. 예를 들어, React Query Devtools를 사용하여 쿼리 상태를 실시간으로 모니터링할 수 있다.

**queryKey의 역할**

- 데이터 식별: queryKey는 쿼리를 고유하게 식별하는 키로 사용된다. 이를 통해 React Query는 어떤 데이터를 페칭하고 캐싱해야 하는지 구분할 수 있다.
- 캐시 관리: 동일한 queryKey를 가진 쿼리는 동일한 데이터를 참조한다. 이를 통해 동일한 데이터를 여러 컴포넌트에서 사용하는 경우에도 중복 페칭을 방지하고, 효율적으로 캐싱할 수 있다.
- 자동 리페칭: queryKey가 변경되면 React Query는 자동으로 데이터를 다시 페칭한다. 이를 통해 데이터가 항상 최신 상태를 유지하도록 한다.
- 데이터 무효화: 특정 queryKey에 해당하는 데이터가 변경되면 이를 무효화하여 최신 데이터를 다시 페칭할 수 있다. 예를 들어, 데이터 수정 후 queryKey를 기준으로 캐시를 무효화하여 최신 데이터를 유지한다.

사용 예시
```tsx
import { useQuery } from 'react-query';

function fetchTodoList() {
    return fetch('/api/todos').then(response => response.json());
}

function TodoList() {
    const { data, error, isLoading } = useQuery(['todos'], fetchTodoList);

    if (isLoading) return 'Loading...';
    if (error) return 'An error has occurred: ' + error.message;

    return (
        <ul>
            {data.map(todo => (
                <li key={todo.id}>{todo.title}</li>
            ))}
        </ul>
    );
}
```
