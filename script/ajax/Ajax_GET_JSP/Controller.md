```java
@GetMapping("/signboard.do")
	@ResponseBody
	public ModelAndView boardAlarm(HttpServletRequest req, HttpServletResponse res) throws Exception {
		
		ModelAndView mv = new ModelAndView();
		Map<String, String> params = new HashMap<>();
		List<LoanSignBoardVo> board = multiFrontService.selectList(params); // vo selectList 
		mv.addObject("loanBoard",board);
		return mv;

	}

```
