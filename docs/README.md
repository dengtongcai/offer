# Welcome read me !

> An awesome project.

:palm_tree:

```java
public static void main(String[] args) {
	SpringApplication.run(ProfilesApplication.class, args);
}
```





```html
<div class="text-center block hidden-xs">
    <script type="text/javascript">
        OA_show(2);
    </script>
</div>
```

```java
public static void main(String[] args) {
    List<Long> obj1 = new ArrayList<>(Arrays.asList(1L, 2L, 3L, 4L));
    List<Long> obj2 = new ArrayList<>(Arrays.asList(5L, 2L, 3L, 4L));

    for (int i = 0; i < obj2.size(); i++) {
        System.err.println(obj2.get(i));
        if (obj1.remove(obj2.get(i))) {
            obj2.remove(i);
            i--;
        }
    }
    System.out.println(obj1);
    System.out.println(obj2);
}
```

