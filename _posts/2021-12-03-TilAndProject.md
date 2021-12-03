---
title: TIL 2021-12-03 2주 TeamProject 와 TIL 
tags: [TIL, JAVA, SPRING, PROJECT]
categories: TIL,PROJECT
---

# 팀프로젝트 01 EMP PROJECT 완성

약 1주일 조금 넘는 시간 동안 팀프로젝트를 진행하였습니다. 저희 팀의 주제는 인적자원 정보 관리와 출결 조회 시스템이였습니다. 

# 저의 역할 
저는 출결 조회 시스템의 백앤드 를 담당 하였습니다. 
그리고 DB와 VO 세팅과 다른 조원 분들 께서 도움이 필요하신 부분을 지원 하였습니다. 


# 이번 프로젝트에서 배운 점 
프로젝트의 완성을 위해 git 을 사용하는 방법을 배웠습니다. 저희 팀은 모두 git을 거의 처음 해봐서 다같이 헤딩하면서 배웠는데, 
처음에는 repository도 계속 생성하고 지우고를 반복하고, project도 계속 생성하고 지우고를 반복 했습니다. 그래도 다같이 zoom으로 어떻게 하는지 하나하나 공부하고 해보고 문제를 해결해가면서 
재밌게 배울 수 있었습니다. 메인 branch에 백업 하기 전에 모두 같이 테스트 코드 돌려보고, 스프링 잘 뜨는지, 내용은 문제없는지 확인하는 것, 그리고 버전 충돌 날 때 줌에 띄워놓고 담당자에게 물어가면서 
선택하는 것도 재밌었습니다. 

그리고 협업을 하는 방법에 대해 배울 수 있었습니다. 
혼자 하는 작업은 모두 자기 혼자 결정 하면 되고, 뒤엎는것도 혼자 결정해서 빠르게 진행 하면 되지만, 여러명이 같이 작업을 하면 이렇게 할 수 없는데, 
문제를 함께 의논해서 결정 하고 진행하는 방법을 배울 수 가 있었습니다. 그리고 서로 조금씩 부족한 부분과 잘하는 부분이 다르기 때문에 모두가 팀에 도움이 된다는 것도 배울 수 있었습니다.


controller와 html 사이에서 데이터를 주고 받는 것, 그러니까 ide 안이 아닌 web으로 넘어가는 것이 사실 아직도 좀 낯설긴한데 좀 더 배울 수 있었습니다. 

# 이번 프로젝트에서 힘들었던 점 
프로젝트 기간의 절반 이상을 각종 세팅을 하는데 사용한 것 같습니다. DB만 해도 서로 다른 PC 환경이라 같은 것으로 바로 설정하기가 어려웠어요. 
한 사람이 되면 다른 사람이 안되거나, 프로젝트 진행하다가 git에 문제가 생겨서 해결해야한다거나 하는 일이 모두 난관 이었습니다. 

그래서 제가 조금만 더 잘 알았었으면 바로 바로 해결 할 수 있었을텐데 하는 생각이 들어서 아쉬웠습니다. 

그리고 인텔리제이에서 springboot 프로젝트를 디버깅하는 방법을 몰라서 이부분이 아쉽고 힘들었습니다. 
repository , service 등은 단위 테스트와 spring 복합 테스트를 아주 작은 단위로 나눠서 할 수 있게 객체를 만들어서 어렵지 않았는데, 
controller 와 html 사이의 일을 디버깅할줄 몰라서, 진짜 무조건 빨리 배워야겠습니다. 

힘든건 아니지만, 아쉬웠던 점은 제가 프로젝트를 기획 하고 설계하는 경험을 더 해봤었으면 좋았을 것 같습니다. 그러면 좀 더 우리 팀이 편하게 퀄리티 좋은 결과를 낼 수 있었을텐데 하는 생각이 들어서 아쉽습니다. 


# 출결 조회 시스템에서 구현한 기능 

- 유저는 출근 시각을 저장 할 수 있고, 퇴근 시각을 저장할 수 있습니다. 
- 그리고 일주일 단위의 출퇴근 기록과 각 일자의 근로 시간을 조회 할 수 있습니다. 
- 또한 일주일의 근로시간과 인정근로시간 그리고 잔여 근로 시간을 조회 할 수 있습니다. 

- 유저가 출퇴근 시간을 처음 기록 하면, 유저의 id 가 기록 됩니다. 
- 이후 수정하게 되면 수정한 사람의 id가 새롭게 저장 됩니다. 


# 코드 모음 

## 필요한 Entity 

### UserVO
```java
@Entity
@Table(name="tbl_user")
public class UserVO {
	@Id
	//@GeneratedValue(strategy = GenerationType.AUTO)
    private String id;
    private String password;
    private int eno ;
    private int egrade; //1: admin, 2: normal 3: retiree
    private LocalDate accdate;
}
```

### TimeStampPK

TimeStamp 의 경우, PK 가 여러개 여서 TimeStampPK 를 작성하였습니다. 
```java
@Data
public class TimeStampPK implements Serializable {

    private	int eno	;//사원번호 pk
    private LocalDate dateTime	;//연/월/일 pk
}
```

### TimeStampVO

```java
@Entity
@IdClass(TimeStampPK.class)
@Table(name = "tbl_timeStamp")
public class TimeStampVO implements Serializable {

    @Id
    private	int eno	;//사원번호 pk
    @Id
    private LocalDate dateTime	;//연/월/일 pk
    private LocalTime startTime	;//출근시간
    private	LocalTime endTime ;	//퇴근시간
    private	int start_editor	;//출근시간등록자
    private	int last_editor ;//	퇴근시간등록자

```

## 필요한 Repository
### TimeStampVORepository

jpa 를 사용했기 때문에 repository 는 아주 간단하게 만들 수 있었습니다.

```java
@Repository
public interface TimeStampVORepository extends JpaRepository<TimeStampVO, Integer> {
    List<TimeStampVO> findByEnoAndDateTime(int i, LocalDate i1);
    List<TimeStampVO> findAllByEnoAndDateTimeBetween(int i, LocalDate i1, LocalDate i2 );
}
```

## 필요한 Service 

### TimeStampService 

service 가 한개만 필요 했기 때문에 class 로 바로 만들었습니다. service 에서 리턴 하는 값들을 계산 하기 위해서 
WorkCalendar, WeeklyHours, WorkingHours 등의 클래스와 인터페이스를 만들었습니다. 



```java
@Service
public class TimeStampService {
private TimeStampVORepository repository;
private WorkingHours workingHours;

    @Autowired
    public TimeStampService(TimeStampVORepository repository, WorkingHoursImpl workingHours) {
        this.repository = repository;
        this.workingHours = workingHours;
    }

    public TimeStampVO getTodayWorkTime(UserVO userVO){
        TimeStampVO timeStampVO = new TimeStampVO();
        List<TimeStampVO> byEnoAndDate = repository.findByEnoAndDateTime(userVO.getEno(), LocalDate.now());
        if (byEnoAndDate.size()!=0) {
            timeStampVO= byEnoAndDate.get(0);
        }
        return  timeStampVO;
    }


    public TimeStampVO recordStartTime(UserVO userVO){
        TimeStampVO stampVO = new TimeStampVO();
        stampVO.setEno(userVO.getEno());
        stampVO.setDateTime(LocalDate.now());
        stampVO.setStartTime(LocalTime.now());
        stampVO.setStart_editor(stampVO.getEno());
        repository.save(stampVO);
        return stampVO;

    }

    public TimeStampVO recordEndTime(UserVO userVO){

        // method 새로 정의 필요
        List<TimeStampVO> byEnoAndDate = repository.findByEnoAndDateTime(userVO.getEno(), LocalDate.now());
        byEnoAndDate.get(0).setEndTime(LocalTime.now());
        byEnoAndDate.get(0).setLast_editor(userVO.getEno());
        repository.save(byEnoAndDate.get(0));
        return byEnoAndDate.get(0);
    }

    public List<TimeStampVO> getWeeklyWorkingHoursData(UserVO userVO,LocalDate first, LocalDate last){

        return repository.findAllByEnoAndDateTimeBetween(userVO.getEno(), first, last);

    }

    public WeeklyHours calWeeklyWorkingHours(List<TimeStampVO> TimeStamp){
        /*WorkingHoursImpl workingHours = new WorkingHoursImpl();*/
        return workingHours.calculateDuration(TimeStamp);
    }

    public Map<LocalDate, Long > calculateDay(List<TimeStampVO> TimeStamp){
        Map<LocalDate, Long> store = new HashMap<>();

        for (TimeStampVO vo :
                TimeStamp) {
            long l = workingHours.calculateDay(vo);
            store.put(vo.getDateTime(),l);
        }
        return store;
    }



}
```

### WorkingHours 의 클래스 들 

#### WeeklyHours

##### WeeklyHours 객체 
특정한 기간의 근로시간을 계산 해서 전달할 값을 담는 객체를 만들었습니다. WorkingHours의 메소드에
UserId와 특정한 기간의 시작일과 말일을 이용해서 database에서 꺼내온 TimeStampVO List를 넣으면, 
총근로시간과 임금 을 청구할 수 있는 근로시간, 그리고 남은 근로 시간을 계산해서 담을 수 있게 할 거에요. 

```java
public class WeeklyHours {
    private Long totalHours;
    private Long wageHours;
    private Long remainHours;

    public Long getTotalHours() {
        return totalHours;
    }

    public void setTotalHours(Long totalHours) {
        this.totalHours = totalHours;
    }

    public Long getWageHours() {
        return wageHours;
    }

    public void setWageHours(Long wageHours) {
        this.wageHours = wageHours;
    }

    public Long getRemainHours() {
        return remainHours;
    }

    public void setRemainHours(Long remainHours) {
        this.remainHours = remainHours;
    }

    @Override
    public String toString() {
        return "WeeklyHours{" +
                "totalHours=" + totalHours +
                ", wageHours=" + wageHours +
                ", remainHours=" + remainHours +
                '}';
    }
}
```



##### WorkingHours interface 
TimeStamp 객체들의 근로시간을 계산해주기 위한 메소드를 담은 객체 입니다. 
인터페이스를 먼저 만들고, 클래스를 만들었습니다. 

사실 인터페이스를 만들 때는 하루의 계산만 하는 calculateDay만 생각 했었는데, 
진행하다보니 calculateDuration(기간단위 리스트 계산)도 여기에 함께 넣게 되었어요. 


```java
public interface WorkingHours {

    long calculateDay(TimeStampVO timeStampVO);
    WeeklyHours calculateDuration(List<TimeStampVO> timeStampVOS);
}
```

```java
@Component
public class WorkingHoursImpl implements WorkingHours{
    long dutyTime = 8;
    LocalTime startLunch = LocalTime.of(12,0,0);
    LocalTime endLunch = LocalTime.of(13,0);
    Long lunchTime = ChronoUnit.HOURS.between(startLunch,endLunch);


    @Override
    public long calculateDay(TimeStampVO timeStampVO) {

        long workingHours;


        if(timeStampVO.getEndTime()==null){
            workingHours = ChronoUnit.HOURS.between(timeStampVO.getStartTime(), LocalTime.now());
        }else {
            workingHours = ChronoUnit.HOURS.between(timeStampVO.getStartTime(), timeStampVO.getEndTime());
        }


        if(timeStampVO.getStartTime()==null || workingHours <=0 ){
            return workingHours;
        }
        return workingHours - lunchTime ;
    }

    @Override
    public WeeklyHours calculateDuration(List<TimeStampVO> timeStampVOS) {
        WeeklyHours weeklyHours = new WeeklyHours();

        long sum = 0;
        int cnt = 0;
        for (TimeStampVO vo :
                timeStampVOS) {
            sum += this.calculateDay(vo);
        }

        weeklyHours.setTotalHours(sum);
        weeklyHours.setWageHours(sum);
        weeklyHours.setRemainHours(dutyTime*5 - sum);
        return weeklyHours;
    }

}
```

##### WorkCalendar 

input 받은 날을 기준으로 해당 주의 월요일과 일요일의 날짜를 알려주는 메소드가 필요했습니다. 
그래서 맵으로 알려주는 클래스를 작성하였습니다. 

```java

@Component
public class WorkCalendar {

    Map<String, LocalDate> dateMap = new HashMap<>();

    public Map<String, LocalDate> week(LocalDate date){
        dateMap.put("Mon",date.with(DayOfWeek.MONDAY));
        dateMap.put("Sun",date.with(DayOfWeek.SUNDAY));

        return dateMap;
    }
}

```

## Controller 

컨트롤러에는 아래와 같이 4개의 메소드를 작성하였습니다. 
- SelfTimeStamp 
  - html에서 javascript로 던지는(?) String Type의 date 와 session에 넣어서 전달되는 userVO 를 전달 받아서, 
  - 현재날짜 기준 월요일, 일요일 까지 기간 처리:  workCalendar의 월요일과 일요일을 받는 메소드를 호출 하여 Map을 받아서 model에 넣고, 
  - 해당 기간 근무 시간 목록을 받아서 model에 넣고,
  - 기간 근로 시간 계산 해서 model에 넣고,
  - 일별 근로시간을 model에 넣었습니다. 
  - 그리고 html을 리턴 합니다! 
  
- startStamp
  - session에 담겨져 있는 UserVO 를 이용해서, timeStampService.recordStartTime 메소드를 호출 해 출근시간을 기록 하고, 
  - redirectAttributes.addFlashAttribute 를 이용해서 userVO를 담아서 SelfTimeStamp2로 redirect 했습니다. 
  - redirect 하면서 객체를 던지는 것을 잘 몰라서 한참 헤맸었어요 ㅠㅠ
  
- endStamp
  - startStamp와 같습니다. 
- SelfTimeStamp2
  - SelfTimeStamp와 같지만, date 를 받지 않는 메소드 입니다. 
    - startStamp와 endStamp 그리고 다른 곳에서 링크로 바로 연결 될 수 있게 하려고 만들었는데, 
    - SelfTimeStamp과 중복 되기 때문에, 이렇게 하지 않고 더 깔끔하게 만들 수 있지 않았을까 하는 생각이 들어서 아쉽습니다. 

```java

@Controller
public class SelfTimeStampController {
    TimeStampService timeStampService;
    WorkingHours workingHours;
    WorkCalendar workCalendar;

    @Autowired
    public SelfTimeStampController(TimeStampService timeStampService, WorkingHours workingHours, WorkCalendar workCalendar) {
        this.timeStampService = timeStampService;
        this.workingHours = workingHours;
        this.workCalendar = workCalendar;
    }
    
    //
    @GetMapping("/SelfTimeStamp")
    //@ModelAttribute
    public String SelfTimeStamp(@RequestParam("newDate") String newDate , Model model, HttpServletRequest request){

        // String date -> local date 객체로 변환하기

        LocalDate date = LocalDate.parse(newDate, DateTimeFormatter.ISO_DATE);

    	HttpSession session = request.getSession(false);
		UserVO userVO = (UserVO)session.getAttribute("user");
        model.addAttribute("user",userVO);

        //현재날짜 기준 월요일, 일요일 까지 기간
        Map<String, LocalDate> week = workCalendar.week(date);
        model.addAttribute("week",week);
        //해당 기간 근무 시간 목록
        List<TimeStampVO> weekly = timeStampService.getWeeklyWorkingHoursData(userVO, week.get("Mon"), week.get("Sun"));
        model.addAttribute("weeklyData",weekly);

        //기간 근로 시간 계산
        Map<LocalDate,Long> DayWorkCal = timeStampService.calculateDay(weekly);
        model.addAttribute("DayWorkCal", DayWorkCal);

        //주간 근로시간 계산
        WeeklyHours weeklyHours = timeStampService.calWeeklyWorkingHours(weekly);
        model.addAttribute("calHours",weeklyHours);

        //일별 근로시간
        TimeStampVO timeStampVO =timeStampService.getTodayWorkTime(userVO);
        model.addAttribute("today", timeStampVO);
        //System.out.println("..222");
        return "selfTimeStamp2";
    }

    @GetMapping("/selfTimeStamp/start")
    public String startStamp(RedirectAttributes redirectAttributes , HttpServletRequest request) throws Exception{
        HttpSession session = request.getSession(false);
        UserVO userVO = (UserVO)session.getAttribute("user");


        timeStampService.recordStartTime(userVO);
        redirectAttributes.addFlashAttribute(userVO);
        return "redirect:/SelfTimeStamp22";
        
    }

    @GetMapping("/selfTimeStamp/end")
    public String endStamp(RedirectAttributes redirectAttributes , HttpServletRequest request) throws Exception{
        HttpSession session = request.getSession(false);
        UserVO userVO = (UserVO)session.getAttribute("user");


        timeStampService.recordEndTime(userVO);
        redirectAttributes.addFlashAttribute(userVO);
        return "redirect:/SelfTimeStamp22";
        
    }



    @GetMapping("/SelfTimeStamp22")
    //@ModelAttribute
    public String SelfTimeStamp2(@ModelAttribute UserVO userVO, Model model){

        model.addAttribute("user",userVO);
        System.out.println("userVO = " + userVO.getEno());


        //현재날짜 기준 월요일, 일요일 까지 기간
        Map<String, LocalDate> week = workCalendar.week(LocalDate.now());
        model.addAttribute("week",week);
        //해당 기간 근무 시간 목록
        List<TimeStampVO> weekly = timeStampService.getWeeklyWorkingHoursData(userVO, week.get("Mon"), week.get("Sun"));
        model.addAttribute("weeklyData",weekly);

        //기간 근로 시간 계산
        Map<LocalDate,Long> DayWorkCal = timeStampService.calculateDay(weekly);
        model.addAttribute("DayWorkCal", DayWorkCal);


        //주간 근로시간 계산
        WeeklyHours weeklyHours = timeStampService.calWeeklyWorkingHours(weekly);
        model.addAttribute("calHours",weeklyHours);

        //일 별 근로시간 계산
        model.addAttribute("today", timeStampService.getTodayWorkTime(userVO));

        return "selfTimeStamp2";
    }

}

```

## HTML 
사실 html 은 다른 팀원들께서 진짜 열심히 만들어주신 것에 값을 넣은 정도에 불과 해서 저는 아무것도 기여하지 못했습니다 ㅠㅠㅠ 
html 너모 어렵습니다. 
그래도 html에 데이터를 던지고, 받으면서 컨트롤러와 연결 하는 것과 th 의 사용법에 대해 많이 배울 수 있었습니다. 


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="./css/selfTimeStamp2.css">
    <script type="module" src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.esm.js"></script>


    <script th:inline="javascript">


        function showClock()
        {
            var currentDate=new Date();
            var divClock=document.getElementById("divClock");
            var apm=currentDate.getHours();
            if(apm<12)
            {
                apm="오전";
            }
            else
            {
                apm="오후";
            }

            var msg = "현재시간 "+"\r"+"\r"+apm +(currentDate.getHours()-12)+"시";
            msg += currentDate.getMinutes() + "분";
            msg += currentDate.getSeconds() + "초";


            divClock.innerText=msg;

            setTimeout(showClock,1000);
        }


        function changeDateBack() {
            //var backday = document.getElementById('backlink');

            var stnDatestr = [[${week.get('Mon')}]];

            var stnDate = new Date(stnDatestr);
            stnDate.setDate(stnDate.getDate() - 7);
            var newDate = stnDate.toISOString().substring(0, 10);



            document.getElementById("backlink").href ="/SelfTimeStamp?newDate=" + newDate;
        }


        function changeDateforward() {
            //var forwardday = document.getElementById('forwardlink');


            var stnDatestr = [[${week.get('Mon')}]];
            var stnDate = new Date(stnDatestr);
            stnDate.setDate(stnDate.getDate() + 7);
            var newDate = stnDate.toISOString().substring(0, 10);


            document.getElementById("forwardlink").href = "/SelfTimeStamp?newDate=" + newDate;
        }

        function toDayLink() {

            var today = new Date();
            var newDate = today.toISOString().substring(0, 10);

            location.href="/SelfTimeStamp?newDate=" + newDate;

        }

    </script>



<!--    CSS / STYLE -->
    <style>
        body {
            margin: 0;
            padding: 0;
        }
        .header {
            padding-bottom: 30px;
            border-bottom: 1px solid black;
        }
        .container {
            width: 980px;
            margin: auto;
        }
        .container-left {
            padding-top: 20px;
            padding-bottom: 20px;
        }
        .logo {
            float: left;
            margin-right: 10px;
        }
        .logo img {
            display: block;
        }
        .menu {
            float: left;
        }

        .custom-btn {
            width: 130px;
            height: 40px;
            color: black;
            border-radius: 5px;
            padding: 10px 25px;
            font-family: 'Lato', sans-serif;
            font-weight: 500;
            background: transparent;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            display: inline-block;
            box-shadow:inset 2px 2px 2px 0px rgba(255,255,255,.5),
            7px 7px 20px 0px rgba(0,0,0,.1),
            4px 4px 5px 0px rgba(0,0,0,.1);
            outline: none;
        }
        .btn-3 {
            width: 130px;
            height: 40px;
            line-height: 42px;
            padding: 0;
            border: none;

        }
        .btn-3 span {
            position: relative;
            display: block;
            width: 100%;
            height: 100%;
        }
        .btn-3:before,
        .btn-3:after {
            position: absolute;
            content: "";
            right: 0;
            top: 0;
            background: black;
            transition: all 0.3s ease;
        }
        .btn-3:before {
            height: 0%;
            width: 2px;
        }
        .btn-3:after {
            width: 0%;
            height: 2px;
        }
        .btn-3:hover{
            background: transparent;
            box-shadow: none;
        }
        .btn-3:hover:before {
            height: 100%;
        }
        .btn-3:hover:after {
            width: 100%;
        }
        .btn-3 span:hover{
            color: rgba(2,126,251,1);
        }
        .btn-3 span:before,
        .btn-3 span:after {
            position: absolute;
            content: "";
            left: 0;
            bottom: 0;
            background: gray;
            transition: all 0.3s ease;
        }
        .btn-3 span:before {
            width: 2px;
            height: 0%;
        }
        .btn-3 span:after {
            width: 0%;
            height: 2px;
        }
        .btn-3 span:hover:before {
            height: 100%;
        }
        .btn-3 span:hover:after {
            width: 100%;
        }
        .clearfix:after {
            content: "";
            display: block;
            clear: both;
        }
        .sidebar {
            background: lightgrey;
            width: 250px;
            height: 900px;
            border-right: 1px solid black;
            float: left;
        }
        .clock {
            width: 140px;
            height: 60px;
            margin-left: 20px;
            border: 1px solid black;
            padding: 10px;
            border-radius: 5px;
        }
        #btn1 {
            background: black;
            color: white;
            width: 100px;
            height: 50px;
            margin-top: 10px;
            margin-left: 10px;
            font-size: 20px;
            border-radius: 10px;
        }
        #btn1:hover {
            background: white ;
            color: black;
            transition: 1s;
        }
        #btn2 {
            background: black;
            color: white;
            width: 100px;
            height: 50px;
            margin-top: 10px;
            margin-left: 10px;
            font-size: 20px;
            border-radius: 10px;
        }
        #btn2:hover {
            background: white ;
            color: black;
            transition: 1s;
        }
        .main_div {
        }

        .calData {
            float: left;
            width: 400px;
            height: 50px;
            margin: 30px 30px;
            border: 1px solid black;
        }

        .icon_btn {
            margin-top: 60px;
        }
        ion-icon{
            font-style: normal;
            font-weight: normal;
            font-variant: normal;
            text-transform: none;
            font-size: 11pt;
            padding: 0px 0px 0px 0px;
        }
        .work_div {
            float: left;
            height: 50px;
            margin-top: 10px;
        }
        .workingBtn {
            padding: 10px;
        }

        .time_tbl {
            float: right;
            border: 1px solid black;
            margin-left: 20px;
            padding: 5px;
        }
        .work_tbl_div {
            display: flex;
            margin-top: 20px;
            justify-content: center;
            align-items: center;
            padding: 30px;
        }

        .working_tbl {
            width: 700px;
        }
    </style>

<!--    CSS / STYLE -->

</head>
<body onload="showClock()">
    <div class="header">
        <div class="container">
            <div class="container-left">
                <div class="logo">
                    <img src="https://heropcode.github.io/GitHub-Responsive/img/logo.svg" alt="logo">
                </div>
                <div class="menu clearfix">
                    <button class="custom-btn btn-3"><span>사원 정보 관리</span></button>
                    <button class="custom-btn btn-3"><span>출결 관리</span></button>
                    <button class="custom-btn btn-3"><span>전 사원 정보</span></button>
                </div>
            </div>
        </div>
    </div>
    <div class="sidebar">
        <br />
        <div id="divClock" class="clock"></div>
        <form  th:action="@{/selfTimeStamp/start}" method="get">
            <input th:if="!${today.startTime}" type="submit" id="btn1" value="출근">
            <input th:if="${today.startTime}" type="submit" disabled value="출근"></form>


        <form th:action="@{/selfTimeStamp/end}" method="get">
            <input th:if="!${today.endTime}" type="submit" id="btn2" value="퇴근">
            <input th:if="${today.endTime}" type="submit" disabled value="퇴근"> </form>


    </div>

    <div class="main_div">
        <div class="calData">
            <tr>

                <td th:text="${week.get('Mon')}">2021-11-22(월)</td>

                <td>~</td>
                <td th:text="${week.get('Sun')}">2021-11-28(일)</td>
            </tr>
        </div>
    </div>
    <div class="icon_btn">

        <a href="" id="backlink" onclick="changeDateBack()"><ion-icon name="caret-back-outline"></ion-icon></a>
        <a href="" id="forwardlink" onclick="changeDateforward()"><ion-icon name="caret-forward-outline"></ion-icon></a>

        <input type="button" value="오늘" onclick="toDayLink()">

    </div>
    <div class="work_div">
        <button class="workingBtn"><a onClick="window.location.reload()" style="cursor: pointer;">근무시간<br/>새로고침</a></button>
        <table class="time_tbl">
            <tr>
                <th>총 근무시간</th>
                <th>인정근무</th>
                <th>잔여근무</th>
            </tr>

            <tr>
                <td th:text="${calHours.getTotalHours()}">0h</td>
                <td th:text="${calHours.getWageHours()}">0h</td>
                <td th:text="${calHours.getRemainHours()}">0h</td>
            </tr>

        </table>
    </div>
    <br />
    <br />
    <br />

    <div class="work_tbl_div">
        <table class="working_tbl" border="1">
            <thead class="thead-dark">
            <tr bgcolor="gray">
                <th scope="col">날짜</th>
                <th scope="col">출근</th>
                <th scope="col">퇴근</th>
                <th scope="col">총근무시간</th>
                <th scope="col">인정근무시간</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each="time: ${weeklyData}">
                <td th:text="${time.getDateTime()}"></td>
                <td th:text="${time.getStartTime()}"></td>
                <td th:text="${time.getEndTime()}"></td>
                <td th:text="${DayWorkCal.get(time.getDateTime())}"></td>
                <td th:text="${DayWorkCal.get(time.getDateTime())}"></td>
            </tr>




            </tbody>
        </table>
    </div>






</body>
</html>
```