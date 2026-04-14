### 서술형 문제 1: 컴파일러의 구조와 인터프리터와의 차이점

> 컴파일러와 인터프리터의 차이점을 유연성과 성능 측면에서 비교하여 설명하고, 컴파일러 프론트엔드와 백엔드의 주요 단계를 순서대로 나열하시오.

**모범 답안**

- **성능 및 유연성 비교**
  - **컴파일러**: 고수준 소스 프로그램을 기계어 형태의 타겟 프로그램으로 미리 번역하여 런타임 오버헤드를 피하고 **공격적인 코드 최적화(Aggressive Code Optimizations)**를 수행할 수 있어 **더 나은 성능(Better Performance)**을 제공합니다.
  - **인터프리터**: 명령줄에서 프로시저를 호출하거나 런타임에 변수를 검사 및 수정할 수 있어 **더 나은 유연성(More Flexibility)**과 **오류 처리**를 제공합니다.

- **컴파일러의 주요 단계**
  1. **컴파일러 프론트엔드 (분석 단계)**
     - 어휘 분석기 (Lexical Analyzer) $\rightarrow$ 구문 분석기 (Syntax Analyzer) $\rightarrow$ 의미 분석기 (Semantic Analyzer) $\rightarrow$ 중간 코드 생성기 (Intermediate Code Generator)
  2. **컴파일러 백엔드 (합성 단계)**
     - 코드 최적화기 (Code Optimizer) $\rightarrow$ 코드 생성기 (Code Generator)

### 서술형 문제 2: 정규 문법의 한계와 구문 트리의 차이

> 정규 문법(RG)이 중첩 구조(예: 괄호 쌍 맞추기)를 표현하지 못하는 이유를 상태(State)와 관련지어 설명하고, 파싱 과정에서 생성되는 파스 트리(Parse Tree)와 이후 단계에 쓰이는 추상 구문 트리(AST)의 차이점을 설명하시오.

**모범 답안**

- **정규 문법의 한계**
  정규 문법을 처리하는 유한 오토마타(FA)는 **기억할 수 있는 상태의 개수가 유한(Finite)**합니다. 무한히 깊어질 수 있는 중첩 구조에서 괄호가 몇 번 열렸는지 개수를 셀(Counting) 메모리 공간(스택)이 없기 때문에 중첩 구조를 표현할 수 없습니다.

- **파스 트리(Parse Tree) vs 추상 구문 트리(AST)**
  - **파스 트리 (Parse Tree)**: 구문 분석 단계에서 만들어지며, 연산자 우선순위를 위한 비단말 기호나 구두점(괄호 등)과 같은 **구체적 구문(Concrete Syntax)** 정보를 모두 포함하여 구조가 복잡합니다.
  - **추상 구문 트리 (AST)**: 파싱 이후 단계(타입 검사, 코드 생성 등)에서 사용되며, 불필요한 구두점이나 키워드 정보를 생략하고 프로그램의 필수 구조만 간결하게 표현한 **추상 구문(Abstract Syntax)** 데이터 구조입니다.

### 서술형 문제 3: LL(1) 문법의 조건 및 좌측 재귀의 문제점

> 어떤 문법이 LL(1) 문법이 되기 위해 $A \rightarrow \alpha \mid \beta$ 규칙이 만족해야 하는 조건을 FIRST와 FOLLOW 집합을 이용해 적고, 좌측 재귀(Left-recursive) 문법이 LL(1) 파서에 적합하지 않은 두 가지 이유를 설명하시오.

**모범 답안**

문법이 LL(1)이 되려면 좌측 재귀가 없어야 하며, 각 대안 규칙에 대해 다음 조건을 만족해야 합니다.

1. $FIRST(\alpha) \cap FIRST(\beta) = \emptyset$ (두 대안의 FIRST 집합에 교집합이 없어야 함)
2. $\alpha \Rightarrow^* \epsilon$ 이면, $FIRST(\beta) \cap FOLLOW(A) = \emptyset$
3. $\beta \Rightarrow^* \epsilon$ 이면, $FIRST(\alpha) \cap FOLLOW(A) = \emptyset$

**좌측 재귀 문법이 부적합한 이유:**

- **FIRST 집합 충돌**: 좌측 재귀 문법(예: `Expr ::= Expr "+" INT | INT`)은 두 대안의 **FIRST 집합이 서로 겹치게 됩니다(Overlapping Cases)**.
- **무한 재귀**: 재귀 하향 파서로 구현할 경우, 입력 토큰을 소비하지 않고 함수가 자기 자신을 계속 호출하여 **무한 재귀(Infinite Recursion)**에 빠지게 됩니다.

### 간단한 코딩 문제 1: NFA를 DFA로 변환하는 Subset Construction

> NFA를 DFA로 변환하는 알고리즘(Subset Construction)에서 핵심 역할을 하는 `Move(s, a)` 함수와 `e-closure(s)` 함수의 동작을 정의하시오. 또한 이 알고리즘이 무한 루프에 빠지지 않고 반드시 종료(Halt)되는 이유 3가지를 적으시오.

**모범 답안**

- **주요 함수 정의**
  - **`Move(s, a)`**: 상태 집합 $s$에서 입력 기호 $a$를 읽었을 때 도달할 수 있는 상태들의 집합을 반환합니다.
  - **`e-closure(s)`**: 상태 집합 $s$에서 입력 기호 없이 $\epsilon$(엡실론) 이동만으로 도달할 수 있는 상태들의 집합을 반환합니다.

- **알고리즘이 반드시 종료되는 이유**
  1. 새로운 상태 집합을 추가하기 전에 **중복 여부(Test Before Adding)**를 미리 검사합니다.
  2. NFA 상태들로 만들 수 있는 **부분집합의 총 개수($2^{|S|}$)**가 유한합니다.
  3. 상태 집합을 저장하는 구조(`Dstates`)에 원소를 추가만 하고 삭제하지 않는 **단조(Monotone) 증가** 특성을 가지기 때문입니다.

### 간단한 코딩 문제 2: 재귀 하향 파서(Recursive Descent Parser) 작성

> 다음은 좌측 재귀가 제거된 수식(Expr)에 대한 EBNF 문법입니다. 1개의 룩어헤드(Token)를 확인하며 트리를 순회하는 재귀 하향 파서의 `parseExpr(self)` 메서드를 Python 코드로 작성하시오.
>
> _제공된 문법:_ `Expr ::= INT ( "+" INT )*`

**모범 답안**

```python
def parseExpr(self):
    # 첫 번째 INT 토큰을 소비합니다.
    self.accept(Token.INT)

    # 룩어헤드(currentToken)가 PLUS 연산자인 동안 반복합니다.
    while self.currentToken.kind == Token.PLUS:
        self.accept(Token.PLUS)
        self.accept(Token.INT)
```

**설명:**
이 코드는 반복문(`while`)을 사용하여 `( "+" INT )*` 부분을 효율적으로 처리합니다. 매번 `currentToken.kind`가 `Token.PLUS`인지 확인하여 **1-토큰 룩어헤드(1-token look-ahead)** 규칙을 충족하며, 좌측 재귀가 제거된 상태이므로 무한 재귀 없이 안전하게 파싱을 수행할 수 있습니다.
