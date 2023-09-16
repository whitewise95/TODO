# 16일
- 에러 발생 
```java
Unable to create unique key constraint (cart_id, design_order_option_id) on table cart_design_order_option: database
column 'design_order_option_id' not found.Make sure that you use the correct column name which depends on the naming
strategy in use (it may not be the same as the property name in the entity, especially for relational types) ...
```

- 원인 
> 나는 보통 컬럼명을 카멜케이스로 작성하고, 자동으로 스네이크케이스로 변환하여 테이블에서 이용하는 것으로 사용중이였다.  
> 해당 필드의 경우 hibernate는 카멜케이스 그대로를 컬럼에서 찾으려 시도하였기 때문에 나타난 현상이였다.
```java
@Getter
@Entity
@Table(uniqueConstraints = @UniqueConstraint(columnNames = {"cart_id", "design_order_option_id"}))
public class CartDesignOrderOption extends BaseDomainWithId {

  @NotNull
	private Long designOrderOptionId;

	/**
	 * 장바구니 정보
	 */
	@NotNull
	@OnDelete(action = OnDeleteAction.CASCADE)
	@ManyToOne(fetch = FetchType.LAZY, optional = false)
	private Cart cart;
}
```

-해결
> `@Column` 를 사용해 `name` 속성에 명시적으로 정의해준다.
```java
	@Column(nullable = false, name = "design_order_option_id")
	private Long designOrderOptionId;
```
