


Controller 란? 
컨트롤러는 들어오는 요청을 처리하고 클라이언트에 응답을 반환합ㄴ디ㅏ. 
컨트롤러는 @Controller 데코레이터로 클래스를 데코레이션하여 정의됩니다.

@Controller('/boards')
export class BoardsController{

}
데코레이터는 인자로  Controller 에 의해서 처리하는 경로를 받습니다. 




Handler 란? 
핸들러는 @Get, @Post, @Delete 등과 같은 데코레이터로 장식된 컨트롤러 클래스 내의 단순한 메서드입니다.

@Controller('/boards')
export class BoardsController{
    @Get()
    getBoards(): string{
        return 'This action returns all boards';
    }
}






Providers 란?

프로바이더는 Nest 의 기본 개념입니다. 대부분의 기본 Nest 클래스는 서비스 , 리포지토리, 팩토리, 헬퍼 등
프로바이더로 취급될 수 있습니다.  Providers의 주요 아이디어는 종속성으로 주입할 수 있다는 것입니다.

즉, 객체는 서로 다양한 관계를 만들 수 있으며 

객체의 인스턴스를 연결하는 기능은 대부분 Nest런타임 시스템에 위임될 수 있습니다.


controller A - Service A, Service B, Service C


controller B <--- @Injectable() 
                   Service B



Service 란 ? 
서비스는 소프트웨어 개발 내의 공통 개념이며, Nest.js , javascript에서만 쓰이는 개념이 아니다
@Injectable 데코레이터로 감싸져서 모듈에 제공되며, 이 서비스 인스턴스는 애플리케이션 전체에서 
사용될 수 있다.

서비스는 컨트롤러에서 데이터의 유효성 체크를 하거나 데이터베이스에 아이템을 생성하는 등의
작업을 한다.







DI (Dependecy Injection) 
- 종속성 주입
컨트롤러에서 서비스를 사용하기 위해서는 종속성을 주입해줘야 한다.

@Controller('board')
export class BoardController{
    constructor(private boardsService: BoardsService) {}

    @Get('/:id')
    getBoardById(@Param('id') id: string): Board{
        return this.boardsService.getBoardById(id);
    }
}

위의 코드를 보면 BoardService 를 Constructor 클래스에서 가져오고(Injected) 있습니다.
그런 후에 Private 문법을 사용하고 있다. 이렇게 해서  boardsService를 정의해서
Controller안에서 사용할 수 있게 만들었다. 이렇게 할 수 있는 이유는 타입스크립트의 기능을 이용해서
종속성을 타입으로 해결할 수 있기 때문입니다.






Provider 등록하기
Provider를 사용하기 위해서는 이것을 Nest에 등록해줘야 합니다.
등록은 module 파일에서 할 수 있습니다. module.파일에 providers 항목 안에 해당 모듈에서
사용하고자 하는  provider를 넣어주면 됩니다.






Model
모델을 정의할 땐 Interface나 Class를 사용한다.
Interface는 변수의 타입만을 체크하고
classes는 변수의 타입뿐만아니라 인스턴스 또한 생성할 수 있다.


export interface Board{
    id: string;
    title: string;
    description: string;
    status: BoardStatus
}

export enum BoardStatus{
    PUBLIC = 'PUBLIC'
    PRIVATE = 'PRIVATE'
}





클라이언트에서 보낸 값들은 핸들러에서 어떻게 가져올까?
@Body body를 이용해서 가져온다.

이렇게 하면 모든 request에서 보내온 값을 가져올 수 있으며, 
하나씩 가져오려면 @Body('title') title  이런 식으로 가져오면 된다.

@Post()
createBoard(@Body() body){
    console.log('body',body)
}

@Post()
createBoard(
    @Body('title') title: string,
    @Body('description') description: string
){
    console.log('title',title)
    console.log('description', description)
    return this.boardService.createBoard();
}