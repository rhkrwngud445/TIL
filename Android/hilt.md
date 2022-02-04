# Hilt

의존성 주입(Dependency Injection)

- 의존성 주입이란?
  - 생성자 또는 메소드등을 통해 외부로부터 생성된 객체를 전달받는 것
- 의존성 주입의 특징
  - 클래스간 결합도를 느슨하게 함
  - 인터페이스 기반으로 설계되며, 코드를 유연하게 한다
  - Stub 또는 Mock 객체를 사용하여 단위테스트를 하기가 더욱 쉬워진다
- 의존성 주입이 없는 코드

```kotlin
class MemoRepository{
	private val db = SQLiteDatabase()

	fun load(id : String){...}
}

fun main(){
	val repository = MemoRepository()
	repository.load("8092")
}
```

- 의존성 주입을 하는 코드
  - 데이터베이스 객체의 생성을 MemoRepository가 책임을 지고 있지 않음
  - 

```kotlin
class MemoRepository(private val db : Database){
	fun load(id : String){...}
}

fun main(){
	val db = SQLiteDatabase()
	val repository = MemoRepository(db)
	repository.load("8092")
}
```

안드로이드에서 의존성 주입이 어려운 이유

- Android 클래스(Activity, Fragment, Service등)가 Framework에 의해 인스턴스화 됨
- Factory를 API 28부터 제공, 하지만 현실적이지 않음

Dagger2

- Dagger2는 자바와 안드로이드를 위한 강력하고 빠른 의존성 주입 프레임워크
- Dagger2 특징
  - 컴파일 타임에 그래프를 구성
  - 생성된 코드는 명확하고 디버깅이 가능
  - 리플렉션 사용x, 런타임 바이트 코드 생성x
  - 자원 공유의 단순화
  - 작은 라이브러리 크기
  - 상위 10000개의 안드로이드 앱 중 74%가 Dagger를 사용
- Dagger2의 단점
  - 배우기 어렵고 프로젝트 설정이 힘들다
  - 간단한 프로그램을 만들 때는 번거롭다
  - 같은 결과에 대해 다양한 방법이 존재한다

의존성 주입 프레임워크의 궁극적인 목표

- 정확한 사용방법을 제안
- 쉬운 설정 방법
- 중요한 것들에 집중할 수 있도록 함

Dagger는 프로젝트 빌드 시 컴파일 타임에 에러 검출

- 장점이기도 하지만 빈번하면 개발자 생산성 저하

Hilt

- Hilt는 애플리케이션에서 DI를 사용하는 표준적인 방법을 제공한다.

- Hilt의 목표

  - Dagger 사용의 단순화
  - 표준화된 컴포넌트 세트와 스코프로 설정과 가독성/이해도 쉽게 만들기
  - 쉬운 방법으로 다양한 빌드 타입에 대해 다른 바인딩 제공

- Hilt 프로젝트 설정

  ```kotlin
  //안드로이드 Gradle 모듈 build.gradle 파일
  dependencies{ 
  implementation'com.google.dagger:hilt-android:<VERSION>' 
  kapt'com.google.dagger:hilt-android-compiler:<VERSION>' 
  }
  ```

- Hilt Gradle Plugin 설정

  ```kotlin
  	// 프로젝트 최상위 build.gradle 파일
  buildscript {
  	repositories {
  		mavenCentral()
  	}
  	dependencies {
  	classpath 'com.google.dagger:hilt-android-gradle-plugin:<version>'
  	}
  }
  
  //안드로이드 Gradle 모듈 build.gradle 파일
  
  apply plugin: 'com.android.application'
  apply plugin: 'dagger.hilt.android.plugin'
  android{...}
  ```

- Hilt 특징

  - Dagger2 기반의 라이브러리
  - 표준화된 Dagger2 사용법을 제시
  - 보일러플레이트 코드  감소
  - 프로젝트 설정 간소화
  - 쉬운 모듈 탐색과 통합
  - 개선된 테스트 환경
  - Android Studio의 지원
    - Android Studio 4.2 이상 버전에서 Hilt object를 가시화된 이미지로 확인 가능
  - AndroidX 라이브러리 호환

- Quick Setup

  - @Inject는 의존성 주입을 받는다는 의미

  ```kotlin
  class MemoRepository @Inject constructor(
  	private val db : MemoDatabase
  ){ 
  	fun load(id : String){...}
  }
  ```

  - Hilt 설정시 선행되어야 함
  - 모든 의존성 주입의 시작점

  ```kotlin
  @HiltAndroidApp
  class MemoApp : Application(){
  	...
  }
  
  @AndroidEntryPoint
  class MemoActivity : AppCompatActivity(){
  	@Inject lateinit val repository : MemoRepository
  
  	override fun onCreate(saveInstanceBundle : Bundle){
  		super.onCreate(bundle)
  		repository.load("8092")
  	}
  }
  ```

  ```kotlin
  @InstallIn(ApplicationComponent::class) 
  @Module 
  object DataModule{ 
  	@Provides 
  	fun provideMemoDB(@ApplicationContextcontext:Context)=
  		Room.databaseBuilder(context,MemoDatabase::class.java,"Memo.db") .build() 
  }
  ```

- 주요 Hilt Annotaion

  - @HiltAndroidApp

    - Hilt 코드 생성을 시작, 반드시 Application class에 추가

    ```kotlin
    @HiltAndroidApp
    class MemoApplication : Application(){ 
    	override fun onCreate(){ 
    	super.onCreate() //의존성주입은super.onCreate()에서이뤄짐(bytecode변환) } 
    }
    ```

  - @AndroidEntryPoint

    - 어노테이션이 추가된 안드로이드 클래스에 DI 컨테이너를 추가
    - @HiltAndroidApp 설정 후 사용 가능
    - @HiltAndroidApp → Component 생성
    - @AndroidEntryPoint → SubComponent 생성
    - AndroidEntryPoin를 지원하는 타입
      - Activity
      - Fragment
      - View
      - Service
      - BroadcastReceiver
      - ContentProvider는 지원x 다른 의존성 주입 방법을 통해 지원 가능

  - @InstallIn

    - Hilt가 생성하는 Di 컨테이너에 어떤 모듈을 사용할 지 가리킨다.

    ```kotlin
    @InstallIn(ActivityComponent::class) 
    @Module 
    object MyModule{ 
    ...
    }
    ```

    - @Module 클래스에 @InstallIn이 없으면 컴파일 에러

      - 모듈 수준 Gradle파일에 아래 코드를 추가하여 검사 비활성화 가능
      - Dagger를 Hilt로 마이그레이션 할 때 필요할 수 있음

      ```kotlin
      android{ 
      	defaultConfig{ 
      		javaCompileOptions{ 
      			annotationProcessorOptions{ 
      				arguments+=["dagger.hilt.disableModulesHaveInstallInCheck":"true"] 
      			} 
      		} 
      	} 
      }
      ```

  - @EntryPoint

    - Hilt가 지원하지 않는 클래스에서 의존성이 필요한 경우

      - ex) ContentProvider, DFM, Dagger를 사용하지 않는 3rd-party 라이브러리 등

    - 특징

      - Interface에서만 사용
      - @InstallIn이 반드시 함께 있어야 함
      - EntryPoints 클래스의 정적 메소드를 통해 그래프에 접근

      ```kotlin
      // EntryPoint 생성
      @EntryPoint 
      @InstallIn(ApplicationComponent::class) 
      interface FooBarInterface{ 
      	fun getBar():Bar 
      } 
      // EntryPoint로접근하기 
      val bar=EntryPoints.get( 
      	applicationContext, 
      	FooBarInterface::class.java 
      	).getBar()
      ```

    - EntryPoint 예제 - ContentProvider

    ```kotlin
    @EntryPoint 
    @InstallIn(ApplicationComponent::class) 
    interface MemoEntryPoint{ 
    	fun getRepository():MemoRepository 
    } 
    
    class MemoProvider:ContentProvider(){ 
    	override fun query(...){ 
    		val entryPoint=EntryPointAccessors.fromApplication(context,MemoEntryPoint::class.java) 
    		val repository=entryPoint.getRepository() 
    		... 
    	} 
    }
    ```

    - EntryPoint 예제 - DFM

    ```kotlin
    @Component(dependencies=[MemoEntryPoint::class]) 
    interface MemoEditComponent{ 
    	fun inject(activity:MemoEditActivity) 
    
    @Component.Builder 
    interface Builder{ 
    	fun context(@BindsInstancecontext:Context):Builder  
    	fun dependencies(entryPoint:MemoEntryPoint):Builder 
    	fun build():MemoEditComponent 
    	} 
    }
    
    class MemoEditActivity:AppCompatActivity(){
    	@Inject 
    	lateinit var repository:MemoRepository 
    	override fun onCreate(savedInstanceState:Bundle?){ 
    		DaggerMemoEditComponent.builder().context(this) 
    		.dependencies(EntryPointsAccessors.fromApplication(applicationContext,MemoEntryPoint::class.java)) .build() 
    		.inject(this) 
    		super.onCreate(savedInstanceState) 
    		... 
    	} 
    }
    ```

- Hilt Component 특징

  - Dagger와 다르게 직접적으로 인스턴스화 할 필요가 없음

  - 표준화된 컴포넌트 세트와 스코프를 제공

  - 컴포넌트들은 계층으로 이루어져 있으며

    하위 컴포넌트는 상위 컴포넌트 의존성에 접근할 수 있다. (=SubComponent)

- Scope Binding

  - 모듈에서 사용되는 Scope 어노테이션은 반드시 installIn에 명시된 컴포넌트와 쌍을 이루는 스코프를 사용해야 한다
  - No Scope
    - 다른 액티비티에서 MemoRepository를 주입 받을 때 다른 인스턴스가 생성 됨

  ```kotlin
  class MemoRepository @Inject constructor( private val db : MemoDatabase 
  ){ 
  	fun load(id:String){...} 
  }
  ```

  - Singleton Scope

  ```kotlin
  @Singleton 
  class MemoRepository @Inject constructor( private val db : MemoDatabase 
  ){ 
  fun load(id:String){...} 
  }
  ```

  - 기본 컴포넌트 바인딩

    - @ApplicationContext 또는 @ActivityContext를 사용하여 적절한 Context를 제공

      받을 수 있다.