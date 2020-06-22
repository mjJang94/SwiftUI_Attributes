# @State와 @Binding
뷰 사이의 상태 프로퍼티이자 원천 자료로서, 어떤 데이터에 대한 영속적인 상태를 저장하고 관찰하는 역할을 수행하는 어트리뷰트이다.
뷰 외부에선 사용할 수 없고 private의 형태로 내부에서만 사용 가능하다.

# ObservedObject와 ObservableObject
뷰 외부의 모델이 가진 원천 자료를 다루기 위한 도구로 제공된다. 특히 참조 타입을 사용하는 경우에 ObservableObject가 사용된다.ObservableObject는 프로토콜 타입이고 AnyObject를 채택하고 있다.
<code>
<pre>
class User: ObservableObject {
  let name = "User Name"
  var score = 0
}

struct ContentView: View{
  @ObservedObject var user: User
  ....
}
</code>
</pre>

# @Published
정확히 어떤 변경 사항을 어느 시점에 뷰에 전달할 것인지 알려줄 때 쓰인다. 즉각적으로 뷰에게 변경된 데이터를 전달한다.

# objectWillChange
@Published처럼 프로퍼티의 변경 시점에 즉시 알리는 것이 아닌, 그 시점을 자신이 정하여 알리고 싶은 경우에 쓴다.
ObervableObject 프로토콜에 선언된 objectWillChange 프로퍼티를 사용하면 되고, 코드는 아래와 같다.
<code>
<pre>
let objectWillChange = ObjectWillChangePublisher()
var score = 0 {
  willset{objectWillChange.send()}
}
</code>
</pre>
send 메소드가 실행되면 변경된 데이터를 반영한다.

# @EnvironmentObject
@ObservableObject와 같은 기능을 수행하면서도 부모뷰가 어떤 값을 가지면 그 자식 뷰들은 직접 전달받지 않더라고 어떤 뷰든지 간에 부모 뷰와 동일한 데이터에 접근할 수 있다.
<code>
<pre>
struct ContentView: View {
  var body: some View{
    SuperView().environmentObject(User())
    }
 }
 
 struct SuperView: View {
    var body: some View{
     SubView()
    }
 }
 
 struct SubView: View {
  @EnvironmentObject var user: User
  
  var body: some View {
  Text(user.name.description)
  }
 }
</code>
</pre>
