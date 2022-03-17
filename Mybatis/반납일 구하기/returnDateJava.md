## 도서 반납일 계산 
- 반납일은 기본 2주지만 반납일(2주)사이에 도서관 정기휴일이 존재할 경우 반납일은 2주 + 정기휴일인 요구사항.
- 따라서 DB에 휴일 데이터를 담고(api제공받음)  [현재날짜] ~ [현재날짜로부터 2주까지의 날짜] 사이에 휴일수를 체크.
- 체크한 휴일 개수와 2주(14일) 을 더하면 됨 
- LocalDateTime 을 사용한다면 쉽게 구할 수 있다.
```java
/**
	 * 반납일안내 & 신청자료 알리미
	 * @param req
	 * @param res
	 * @return
	 * @throws Exception
	 */
	@GetMapping("/reference_room3.do")
	public ModelAndView referenceRoom3(HttpServletRequest req, HttpServletResponse res) throws Exception {

		//온라인 열람신청 알림
		ModelAndView mv = new ModelAndView();
		Map<String, String> params = new HashMap<>();
		params.put("loanFinishYn", "N");
		List<LoanSignBoardVo> board = multiFrontService.selectList(params);
		
		//오늘날짜 처리 
		Date today = new Date();
		SimpleDateFormat year, month, day, dayOfWeek, df;
		df = new SimpleDateFormat("yyyy-MM-dd");
		year = new SimpleDateFormat("yyyy");
		month = new SimpleDateFormat("MM");
		day = new SimpleDateFormat("dd");
		dayOfWeek = new SimpleDateFormat("E");
		
		String years = year.format(today);
		String months = month.format(today);
		String days = day.format(today);
		String dayOfWeeks = dayOfWeek.format(today);
				
		//현재 날짜
		mv.addObject("year", years);
		mv.addObject("month",months);
		mv.addObject("days", days);
		mv.addObject("dayOfWeek", dayOfWeeks);
		
		//휴관일 가져오기 
		Map<String, String> holiday = new HashMap<>();
		holiday.put("hd_year", year.format(today).toString());
		holiday.put("hd_month", month.format(today).toString());
		List<HolidayVo> holidays = multiFrontService.selectHolydayList(holiday);
		
		//현재 날짜
		LocalDateTime currentDateTime = LocalDateTime.now();
		
		// 2주 더하기(14 +1 일 더하기 (DB에서 2주째 되는 날까지 포함시키기 위해))
		LocalDateTime plusDays = currentDateTime.plusDays(15);	
		
		String currentDateToString = ((currentDateTime.toString()).split("T"))[0];
		String plusDaysToString = ((plusDays.toString()).split("T"))[0];
		
		Map<String, String> count = new HashMap<>();
		count.put("startDate", currentDateToString);
		count.put("endDate", plusDaysToString);
		int holidayNum = holidayService.selectHolydayCount(count);
		
		//반납일
		LocalDateTime resultReturnDate = currentDateTime.plusDays(14+holidayNum);
		
		int y = resultReturnDate.getYear();
		int m = resultReturnDate.getMonthValue();
		int d  = resultReturnDate.getDayOfMonth();
		DayOfWeek w = currentDateTime.plusDays(14+holidayNum).getDayOfWeek();
		
		mv.addObject("returnYear", y);
		mv.addObject("returnMonth", m);
		mv.addObject("returnDays", d);
		mv.addObject("returnDayOfWeek", w.toString());
		
		mv.addObject("loanBoard", board);	
	
		return mv;
	}

```
