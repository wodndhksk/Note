```javascript
/*******************************************************************************
 * 공통코드 관리
 *
 */
(function() {

	var _category = shop.object.createNestedObject(window,
		"shop.biz.system.category");
	/**
	 * 공통코드 관리
	 */

	/**
	 * 카테고리 그리드
	 */
	_category.initCategorySheet = function() {
		// Grid 객체 생성
		createIBSheet2(document.getElementById("categoryGrid"), "categoryGridSheet");

		var initCategorySheet = {};
		initCategorySheet.Cfg = {SearchMode : 1, DeferredVScroll : 1,  UseHeaderSortCancel : 1,OnePageSort:1, MaxSort: 3};
		initCategorySheet.HeaderMode = {Sort : 1, ColMove : 0, ColResize : 1};
		initCategorySheet.Cols = [
			  {Header : "",				Type : "Status",	SaveName : "status",		Width : 0,	Align : "",			Edit : 0,	Hidden : 1}
			,{Header : "카테고리번호",	Type : "Text",		SaveName : "ctgrNo",		Width : 15,	Align : "Left",		Edit : 1, Hidden : 1}
			,{Header : "카테고리명",		Type : "Text",		SaveName : "lngCdKo",		Width : 25,	Align : "Center",	Edit : 1}
			,{Header : "노출영역",		Type : "Text",		SaveName : "dlvSvcCd",	Width : 12,	Align : "Center",	Edit : 1,}
		];

		// Grid 초기화
		IBS_InitSheet(categoryGridSheet, initCategorySheet);
		// Grid 건수 정보 표시
		categoryGridSheet.SetCountPosition(3);
		// Grid width 자동 조절
		categoryGridSheet.FitColWidth();
		// Grid 마지막 컬럼을 All 너비에 맞춘다.
		categoryGridSheet.SetExtendLastCol(1);
		// 편집 가능 여부 설정
		categoryGridSheet.SetEditable(false);
		// 카테고리명 언더라인
		categoryGridSheet.SetColFontUnderline('lngCdKo', 1);

		this.doAction("readCategory");
	}

	/**
	 * 카테고리품목 그리드
	 */
	_category.initCategorySubSheet = function() {
		// Grid 객체 생성
		createIBSheet2(document.getElementById("categorySubGrid"), "categorySubGridSheet");
		var initCategorySubSheet = {};
		initCategorySubSheet.Cfg = {SearchMode : 1, DeferredVScroll : 1,  UseHeaderSortCancel : 1,OnePageSort:1, MaxSort: 3, MouseHoverMode: 0,};
		initCategorySubSheet.HeaderMode = {Sort : 1, ColMove : 0, ColResize : 1};
		initCategorySubSheet.Cols = [
			  {Header : "",				Type : "Status",	SaveName : "status",		Width : 0,	Align : "",				Edit : 0,	Hidden : 1}
			, {Header : "",				Type: "CheckBox",	SaveName : "ctgrChk", 		Width : 6, 	Align : "Center", 		Edit : 1}
			, {Header : "품목번호",		Type : "Text",		SaveName : "pdltNo",		Width : 15,	Align : "Center",		Edit : 0}
			// ,{Header : "카테고리번호",	Type : "Text",		SaveName : "ctgrNo",		Width : 15,	Align : "Left",			Edit : 1}
			, {Header : "품목명",		Type : "Text",		SaveName : "lngCdKo",		Width : 25,	Align : "Center",		Edit : 1 , KeyField:true}
			, {Header : "품목영문명",		Type : "Text",		SaveName : "lngCdEn",		Width : 25,	Align : "Center",		Edit : 1 , KeyField:true}
			// , {Header : "배송여부",		Type : "Combo",		SaveName : "useYn",			Width : 12,	Align : "Center",		Edit : 1,	ComboText : "전체|일반배송|빠른배송",	ComboCode : "전체|일반배송|빠른배송"}
			, {Header : "정렬순서",		Type : "Int",		SaveName : "sortSeq",		Width : 8,	Align : "Center",		Edit : 1,  KeyField:true}
			, {Header : "사용여부",		Type : "Combo",		SaveName : "useYn",			Width : 12,	Align : "Center",		Edit : 1,	ComboText : "사용|사용안함",	ComboCode : "Y|N"}
			, {Header : "HSCODE",		Type : "Text",		SaveName : "hsCode",		Width : 12,	Align : "Center",		Edit : 1}
			, {Header : "관세",			Type : "Text",		SaveName : "trfRat",		Width : 8,	Align : "Center",		Edit : 1,  KeyField:true}
			, {Header : "특소세",		Type : "Text",		SaveName : "spclRat",		Width : 8,	Align : "Center",		Edit : 1}
			, {Header : "기준초과금",		Type : "Text",		SaveName : "basExcsAmt",	Width : 8,	Align : "Center",		Edit : 1}
			, {Header : "교육세",		Type : "Text",		SaveName : "eduRat",		Width : 8,	Align : "Center",		Edit : 1}
			, {Header : "농특세",		Type : "Text",		SaveName : "villageRat",	Width : 8,	Align : "Center",		Edit : 1}
			, {Header : "주세",			Type : "Text",		SaveName : "liquRat",		Width : 8,	Align : "Center",		Edit : 1}
			, {Header : "통관구분코드",	Type : "Combo",		SaveName : "cleaCatCd",		Width : 8,	Align : "Center",		Edit : 1,  ComboText : "일반|목록",	ComboCode : "일반|목록", KeyField:true}
		];
		// Grid 초기화
		IBS_InitSheet(categorySubGridSheet, initCategorySubSheet);
		// Grid 건수 정보 표시
		categorySubGridSheet.SetCountPosition(3);
		// Grid width 자동 조절
		categorySubGridSheet.FitColWidth();
		// Grid 마지막 컬럼을 All 너비에 맞춘다.
		categorySubGridSheet.SetExtendLastCol(1);
		// drag 시 행 또는 셀 drag 모드 설정
		categorySubGridSheet.SetDragMode(1);
		// Grid Sort 기준컬럼 : 정렬순서(sortSeq)
		categorySubGridSheet.ColumnSort("6", "asc")
		// Grid 공통코드 ComboText 설정
		categorySubGridSheet.CellComboItem(2, 3, {
			"ComboCode": "A001|A002|A003|A004",
			"ComboText": "부장|과장|대리|사원"
		});

		// console.log("정렬 SaveName:", categorySubGridSheet.ColSaveName(6));

		this.doAction("readCategorySub");
	}

	_category.doAction = function(sAction) {
		switch (sAction) {
			// 조회

			case "readCategory":
				var param = {
					url : "/system/category/read-useY-list",
					rowsPerPage : 500,
					subparam : FormQueryStringEnc(document.frmSearch),
					sheet : "categoryGridSheet"
				};
				DataSearchPaging(param);

				break;

			case "readCategorySub":
				var param = {
					url : "/system/category-subject/read-list",
					rowsPerPage : 500,
					subparam : FormQueryStringEnc(document.frmSearch),
					sheet : "categorySubGridSheet"
				};
				DataSearchPaging(param);

				break;

			case "searchCategorySub":
				var param = {
					url : "/system/category-subject/search-list",
					rowsPerPage : 500,
					subparam : FormQueryStringEnc(document.frmSearch),
					sheet : "categorySubGridSheet"
				};
				DataSearchPaging(param);

				break;
		}
	}

	categoryGridSheet_OnClick  = function(Row, Col, Value) {
		if ( Row != 0) {
			var codeCellEditEnb = categoryGridSheet.GetCellEditable(Row, 'lngCdKo');
			if ( categoryGridSheet.ColSaveName(Col) == "lngCdKo" && Value != "" ) {
				var ctgrNo = categoryGridSheet.GetCellValue(Row, "ctgrNo");
				$("#ctgrNo").val(ctgrNo);
				var check = $("#ctgrNo").val();
				console.log("input ctgrNo 값 : ",check);
				shop.biz.system.category.doAction("readCategorySub");
			}
		}
	}

	//카테고리 품목페이지 처음 load시 첫번째 카테고리 정보에 관한 품목 데이터 노출
	categoryGridSheet_OnSearchEnd = function(Code, Msg, StCode, StMsg, Response) {
		//여기서 #catgr 의 val 값 getCellText로 넣기 => categoryGridSheet.GetCellText('1', "ctgrNo"));
		var setCtgrNo = categoryGridSheet.GetCellText('1', "ctgrNo");
		$("#ctgrNo").val(setCtgrNo);

		//카테고리품목 그리드 초기화
		shop.biz.system.category.initCategorySubSheet();
	}

	$(document).ready(function() {
		//카테고리 그리드 초기화
		shop.biz.system.category.initCategorySheet();

		// [검색] HS_CODE 로 카테고리 품목 조회
		$("#srcHscode").off().on("click", function() {
			var srcValue = $('#hsCodeSrc').val();
			if(srcValue === ""){
				alert("값을 입력해주세요~");
			}else{
				$('#addCol').attr('disabled', true);
				$('#delCol').attr('disabled', true);
				shop.biz.system.category.doAction("searchCategorySub");
			}
		});

		// 카테고리품목 행추가
		$(".insertCategory").on("click", function() {
			// 상단에 행 생성 ( 상단생성 : DataInsert(0), 하단생성 : DataInsert(-1))
			var noIdx = categorySubGridSheet.DataInsert(0);
			categorySubGridSheet.SetRowData(noIdx, {foUseYn:'N'});
		});

		// 카테고리품목 행삭제
		$(".deleteCategory").on("click", function() {
			var sRow = categorySubGridSheet.FindCheckedRow("ctgrChk").split('|');
			var deletRowNum = categorySubGridSheet.FindCheckedRow("ctgrChk");
			var message = "";

			for(var i=0; i<sRow.length; i++){
				var pdltNo = categorySubGridSheet.GetCellText(sRow[i], "pdltNo");
				if(pdltNo != ""){
					message += pdltNo + " ";
				}
			}
			message = message.trimEnd();
			message = message.replaceAll(" ", ", ")

			if (message === "") {
				if (sRow == "") {
					alert("삭제할 코드그룹을 선택해 주세요.");
					return false;
				} else {
					if (confirm("삭제 하시겠습니까?")) {
						$.each(sRow, function (i, chkRow) {
							console.log(deletRowNum);
							categorySubGridSheet.RowDelete(deletRowNum);
						});
					}
				}
			}else {
				alert(`카테고리 번호 [ ${message} ] 값은 삭제할 수 없습니다.`);
			}
		});

		// 카테고리품목 저장
		$(".saveCategory").on("click", function() {
			var value = $('#ctgrNo').val();
			console.log(value);
			if (confirm("변경사항을 저장 하시겠습니까?")) {
				var checkedRows = categorySubGridSheet.FindCheckedRow("ctgrChk").split('|');
				for(var i in checkedRows) {
					var currentRow = checkedRows[i];
					categorySubGridSheet.SetRowData(currentRow, { checked : 0 });
				}
				var param = {
					url : "/system/category-subject/save",
					subparam : FormQueryStringEnc(document.frmSearch),
					sheet : "categorySubGridSheet"
				}
				DataSave(param);
				// location.reload();
			}
		});
	});
})();


```
