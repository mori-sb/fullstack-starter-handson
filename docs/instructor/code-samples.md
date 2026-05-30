# Code Sample Plan

概念図の後に見せる短いコードサンプルをまとめる。

コードは長くしすぎず、「どの役割のコードか」が分かることを優先する。

## Spring Boot

### Controller

役割:

APIの入口。URL、HTTPメソッド、リクエスト、レスポンスを扱う。

```java
@RestController
@RequestMapping("/api/restaurants")
public class RestaurantController {

    private final RestaurantService restaurantService;

    public RestaurantController(RestaurantService restaurantService) {
        this.restaurantService = restaurantService;
    }

    @GetMapping
    public List<RestaurantResponse> findAll() {
        return restaurantService.findAll();
    }

    @PostMapping
    public RestaurantResponse create(@RequestBody RestaurantRequest request) {
        return restaurantService.create(request);
    }
}
```

見るポイント:

- `@RestController` がAPI用のControllerを表す
- `@RequestMapping` が共通URLを表す
- `@GetMapping` が一覧取得API
- `@PostMapping` が登録API
- Controllerは細かい処理を持たず、Serviceに渡す

### Request DTO

役割:

Reactから送られてくるJSONの形。

```java
public class RestaurantRequest {
    private String name;
    private String area;
    private String genre;
    private String memo;
    private String imageUrl;
    private RestaurantStatus status;
}
```

見るポイント:

- 画面の入力フォームと近い形になる
- DB用ではなく、APIで受け取るための形

### Response DTO

役割:

Reactへ返すJSONの形。

```java
public class RestaurantResponse {
    private Long id;
    private String name;
    private String area;
    private String genre;
    private String memo;
    private String imageUrl;
    private RestaurantStatus status;
}
```

見るポイント:

- Reactが画面表示に使う形
- `imageUrl` があるのでReactで画像表示できる

### Service

役割:

アプリの処理を書く場所。登録、編集、削除などの中心になる。

```java
@Service
public class RestaurantService {

    private final RestaurantRepository restaurantRepository;
    private final RestaurantMapper restaurantMapper;

    public RestaurantService(
            RestaurantRepository restaurantRepository,
            RestaurantMapper restaurantMapper
    ) {
        this.restaurantRepository = restaurantRepository;
        this.restaurantMapper = restaurantMapper;
    }

    public RestaurantResponse create(RestaurantRequest request) {
        Restaurant restaurant = restaurantMapper.toEntity(request);
        Restaurant saved = restaurantRepository.save(restaurant);
        return restaurantMapper.toResponse(saved);
    }
}
```

見るポイント:

- Controllerから呼ばれる
- Repositoryを使ってDB保存する
- Mapperを使ってDTOとEntityを変換する

### Mapper

役割:

DTOとEntityを変換する。

```java
@Component
public class RestaurantMapper {

    public Restaurant toEntity(RestaurantRequest request) {
        Restaurant restaurant = new Restaurant();
        restaurant.setName(request.getName());
        restaurant.setArea(request.getArea());
        restaurant.setGenre(request.getGenre());
        restaurant.setMemo(request.getMemo());
        restaurant.setImageUrl(request.getImageUrl());
        restaurant.setStatus(request.getStatus());
        return restaurant;
    }

    public RestaurantResponse toResponse(Restaurant restaurant) {
        return new RestaurantResponse(
                restaurant.getId(),
                restaurant.getName(),
                restaurant.getArea(),
                restaurant.getGenre(),
                restaurant.getMemo(),
                restaurant.getImageUrl(),
                restaurant.getStatus()
        );
    }
}
```

見るポイント:

- API用のDTOとDB用のEntityを直接混ぜない
- 変換処理を1か所に集める

### Entity

役割:

DBに保存するデータの形。

```java
@Entity
public class Restaurant {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String area;
    private String genre;
    private String memo;
    private String imageUrl;

    @Enumerated(EnumType.STRING)
    private RestaurantStatus status;
}
```

見るポイント:

- DBテーブルに近い形
- `id` はDB側で採番される
- `imageUrl` は画像そのものではなくURL文字列

### Repository

役割:

DB操作を担当する。

```java
public interface RestaurantRepository extends JpaRepository<Restaurant, Long> {
    List<Restaurant> findByArea(String area);
    List<Restaurant> findByGenre(String genre);
    List<Restaurant> findByStatus(RestaurantStatus status);
}
```

見るポイント:

- `JpaRepository` を継承すると基本的なCRUDが使える
- メソッド名から検索処理を作れる

## React

### API Client

役割:

Spring Boot APIを呼ぶ処理をまとめる。

```jsx
const API_BASE_URL = "http://localhost:8080/api/restaurants";

export async function fetchRestaurants() {
  const response = await fetch(API_BASE_URL);
  return response.json();
}

export async function createRestaurant(restaurant) {
  const response = await fetch(API_BASE_URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(restaurant),
  });
  return response.json();
}
```

見るポイント:

- ReactからSpring Bootを呼ぶ場所
- JSONを送るときは `Content-Type` を指定する
- JavaScriptオブジェクトを `JSON.stringify` でJSON文字列にする

### RestaurantCard

役割:

お店1件分を表示する。

```jsx
export function RestaurantCard({ restaurant }) {
  return (
    <article className="restaurant-card">
      <img src={restaurant.imageUrl} alt={restaurant.name} />
      <h3>{restaurant.name}</h3>
      <p>{restaurant.area} / {restaurant.genre}</p>
      <p>{restaurant.memo}</p>
      <span>{restaurant.status}</span>
    </article>
  );
}
```

見るポイント:

- `props` で `restaurant` を受け取る
- `imageUrl` を `img` の `src` に渡す
- 1件分の表示だけに集中する

### useEffect

役割:

画面表示時にAPIを呼び、取得したデータをstateに入れる。

```jsx
useEffect(() => {
  async function loadRestaurants() {
    const data = await fetchRestaurants();
    setRestaurants(data);
  }

  loadRestaurants();
}, []);
```

見るポイント:

- 画面が表示されたタイミングでAPIを呼ぶ
- `setRestaurants` でstateを更新する
- stateが変わると画面が再描画される
