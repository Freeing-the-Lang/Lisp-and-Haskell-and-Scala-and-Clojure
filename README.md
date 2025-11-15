# Lisp-and-Haskell-and-Scala-and-Clojure — MetaVM Edition

**함수형 언어 4대 천왕(Lisp + Haskell + Scala + Clojure)을 하나로 융합한 실험 언어.**  
이 언어는 **C++ 메타프로그래밍 기반 VM(MetaVM)** 위에서 실행됩니다.  
정적/동적/매크로/타입/패턴매칭/람다/지연평가—all in one.

---

## 🚀 Concept

이 프로젝트는 다음을 결합합니다:

- **Lisp** → S-Expression, 하이퍼 매크로, 구조 중심 문법  
- **Haskell** → 타입 추론, 패턴 매칭, 함수형 순수성  
- **Scala** → 모나딕 스타일, 함수형+객체 혼합 시맨틱  
- **Clojure** → 동적 구조, 데이터 우선 설계, REPL-friendly 실행  
- **C++20/23 메타프로그래밍** → VM 레벨에서 AST를 직접 생성·최적화  
- **Runtime: MetaVM** → C++ 템플릿/constexpr 기반 AST 인터프리터

결과적으로 “하이브리드 함수형 언어”를  
**컴파일러 없이, C++ 자체가 VM이 되어 실행**시킵니다.

---

## 🧠 MetaVM: C++로 만든 함수형 VM

MetaVM은 다음 특징을 갖습니다:

- AST 구조를 C++ 템플릿 타입으로 저장  
- 실행은 `constexpr` + 템플릿 재귀로 평가  
- 런타임은 C++ 객체로 표현된 Closure 기반  
- 매크로는 C++ 메타프로그래밍으로 치환  
- 패턴매칭은 `std::variant` 기반  
- S-Expression 파서는 C++에서 직접 구현

즉, “컴파일될 때 이미 코드가 VM 안으로 들어감”.

---

## 🔥 Example 1 — Lisp Style + Pattern Matching

```lisp
(match (list 1 2 3)
  ((1 _ _) "starts with 1")
  (_        "other"))



MetaVM 내부에서는 C++ 템플릿:


using program =
  match<
    list<int<1>, int<2>, int<3>>,
    case_<pat<1, wildcard, wildcard>, str<"starts with 1">>,
    case_<pat<wildcard>, str<"other">>
  >;




🔥 Example 2 — Haskell Style Lambda


f = \x -> x + 1
print (f 10)



MetaVM 내부 C++ 표현:


using program =
  call<lambda<plus<arg<0>, int<1>>>, int<10>>;




🔥 Example 3 — Scala-like Expression + Monads


for {
  x <- Just(10)
  y <- Just(x + 5)
} yield y * 2



C++ MetaVM:


using program =
  bind<just<int<10>>,
    lambda<bind<just<plus<int<10>, int<5>>>,
      lambda<mul<arg<0>, int<2>>>
    >>
  >;




🏗 Project Structure


src/
  metavm/        # C++20 meta-programming VM core
  parser/        # Lisp/Haskell hybrid parser
  runtime/       # Closure, eval loop, pattern matcher
  examples/
include/
  metavm.hpp
README.md




⚙️ Build


g++ -std=c++23 -O2 -Iinclude src/main.cpp -o metavm
./metavm



Clang++ 16+ 권장.



🎯 목표




함수형 언어 최종 융합 모델 정의


C++ 메타프로그래밍 기반 “컴파일·런타임 일체형 VM”


타입 시스템 + S-expression + 매크로 + 패턴매칭을 하나의 언어로


“컴파일러 없는 언어”라는 새로운 패러다임 실험





📌 Status


초기 프로토타입

MetaVM 코어 확장 중

패턴매칭 · 모나드 · 매크로 단계적으로 추가 예정



License


MIT License



---
