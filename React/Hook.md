### useState

```react
const [ value, setValue ] = useState(initialValue);
```

- 클래스에서 인스턴스 생성하는 것과 비슷하다고 이해
- value는 값이고 setValue는 그 value를 설정할 수 있는 메서드이다.

```react
const increaseValue = () => setValue(value + 1);
```

- useState를 통해서 setValue는 value를 설정할 수 있는 메서드가 되었다.
  ( value와 setValue 이름은 무엇이든 가능하다는 뜻이기도 하다. )



```react
class App extends React.Component{
    state = {
        value: 1
    }
	render() {
    	const { value } = this.state;
        increaseValue = () => {
            this.setState(current =>{
                return {
                	value: current.value + 1;    
                }
            });
        }
        
        return ( //blabla );
            
    }
}
```

- 원래의 react 코드로 바꾸면 이렇게 길어진다.



#### useInput ( hooks func 만들기 )

```react
const useInput = initialValue => {
    const [ value, setValue ] = useState(initialValue);
    const onChange = event => {
        // blabla
    }
    return { value, onChange };
};

const App = () => {
    const name = useInput("Mr.");
    return (
    	<div>
        	<input placeholder="Name" 
                value={name.value} onChagne={name.onChange} />
        </div>
    )
}
```

- 새로운 function을 만들어서 사용할 수도 있다. 
- react 외부에서 새로운 func을 가져다 쓸 수 있다는 점이 중요하다.
- 새롭게 만든 func은 Java에서 클래스 인스턴스하듯이 사용한다.
- {name.value}등은 {...name}으로도 사용이 가능하다.
  ( 이러면 어차피 value, onChange라는 이름으로 설정되어 있기 때문에 value, onChange로 사용됨. )
- setValue는 setState처럼 re-render 한다.





### useEffect

- componentWillUnmount, componentDidMount, componentWillUpdate의 역할을 한다.

```react
const sayHello = () => console.log("Hello");
useEffect(() => {
	sayHello();
})
```

- 위와 같은 경우 sayHello는 언제 실행될까?
- componentDidMount, componentWillUpdate 의 모든 역할을 하기 때문에 component가 mount 되었을 때, update되었을때 모두 실행된다.

```react
useEffect(sayHello, [number]);
```

- 위와 같은 방식으로도 사용할 수 있다.
- useEffect의 첫번째 인자는 func으로 effect 된다.
- 두번째 인자는 해당 값이 update될 때만 첫번째 인자가 실행되도록 한다.
  ( componentDidMount는 별도로 실행된다. )
- 두번째 인자에 빈 dependency를 전달하면 update시에는 func이 실행되지 않는다.

```react
useEffect(() => {
    // ...
    return () => {
        // blabla
    }
}, [])
```

- dependency가 없기 때문에 update될 때 useEffect가 실행되지 않는다.
- useEffect의 return 값은 componentWillUnmount일 때 호출 된다.