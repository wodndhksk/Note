```html
<!-- 다국어 : 배송신청팔아 코드 적용 -->
<template>
  <div class="lp_body">
    <div class="layout">
      <dl class="box_wrap type-border-col-info m_t_16">
        <dt><i>{{ $t('ML60509') }} : </i>{{ buyerInfo.reqNo }}</dt>
        <dd>{{ $t('ML60510') }}: {{ buyerInfo.reqDate }}</dd>
        <dd class="towner">
          {{ $t('ML60511') }} : <span class="badge19">
            <span :class="['badge', `flag-${buyerInfo.flag}`]">{{ buyerInfo.custNm }}</span>
          </span>
        </dd>
      </dl>

      <div class="m_t_16">
        <comp-accordion wrapper-class="type-default">
          <template #accordion-trigger>
            {{ $t('ML60512') }}
          </template>
          <template #accordion-content>
            <div class="memo">
              <p>
                나만의 메모 내용나만의 메모 내용나만의 메모 내용나만의 메모 내용나만의 메모 내용나만의 메모 내용나만의 메모 내용나만의 메모 내용
              </p>
              <a
                href="https://www.gmarket.co.kr"
                target="_blank"
              >
                www.gmarket.co.kr
              </a>
              <a
                href="https://www.gmarket.co.kr"
                target="_blank"
              >
                www.gmarket.co.kr
              </a>
            </div>
          </template>
        </comp-accordion>
      </div>
      <div class="m_t_32">
        <div class="form-box">
          <label>{{ $t('ML60513') }}</label>
          <div class="multiForm type-select-input">
            <div class="multi-item row1">
              <comp-select
                :option-list="shopMallOptList"
                :message="validationReturnInfo.shopmallUrlAutoSn.message"
                :is-error="validationReturnInfo.shopmallUrlAutoSn.isError"
                :value="buyerInfo.shopmallUrlAutoSn"
                @selectOptionClick="handleShoppingMallUrlClick($event)"
              />
            </div>
            <div class="multi-item row2">
              <comp-input
                v-model="shoppingMallUrl"
                :value="buyerInfo.shopmallUrl"
                :message="validationReturnInfo.shopmallUrl.message"
                :is-error="validationReturnInfo.shopmallUrl.isError"
                :placeholder="$t('ML60514')"
                :readonly="shopMallUseYn.yn"
                @input="handleShoppingMall($event)"
              />
            </div>
          </div>
        </div>
      </div>
      <div
        v-for="(item, i) in items"
        :key="i"
        class="list_wrap type-product-info m_t_40"
      >
        <div class="tit">
          <h3>{{ $t('ML50266', {num: i + 1}) }}</h3>
          <div>
            <a
              href="javascript:;"
              class="btn-load"
              @click="$changePage('deliveryAgency', 'LoadPreviousItem')"
            >{{ $t('ML50754') }}</a>
            <a
              v-if="i > 0"
              href="javascript:;"
              class="btn-close"
              @click="() => handleItemDeleteClick(i)"
            >닫기</a>
          </div>
        </div>
        <div class="m_t_32">
          <div class="form-box">
            <label>{{ $t('ML60515') }}</label>
            <div class="multiForm type-select-input">
              <div class="multi-item row1">
                <comp-select
                  :option-list="hdsAutoSnOptList"
                  :message="validationReturnInfo.hdsAutoSn[i].message"
                  :is-error="validationReturnInfo.hdsAutoSn[i].isError"
                  @selectOptionClick="handlehdsAutoSnClick($event, i)"
                />
              </div>
              <div class="multi-item row2">
                <comp-input
                  v-model="forwardingAgent"
                  :message="validationReturnInfo.dlvTrackNo[i].message"
                  :is-error="validationReturnInfo.dlvTrackNo[i].isError"
                  @input="handleDlvTrackNo($event, i)"
                />
              </div>
            </div>
            <p
              class="message"
            >
              <img
                src="@assets/images/icon-error-lined@2x.png"
                width="16"
              >{{ $t('ML60516') }}
            </p>
          </div>
          <div class="form-box m_t_20">
            <comp-select
              :title="$t('ML60517')"
              :option-list="categoryOptList"
              :message="validationReturnInfo.comodityCatAutoSn[i].message"
              :is-error="validationReturnInfo.comodityCatAutoSn[i].isError"
              @selectOptionClick="handleCategoryClick($event, i)"
            />
          </div>

          <div class="form-box m_t_20">
            <comp-select
              :title="$t('ML60518')"
              :option-list="comodityAutoSnArray[i]"
              :message="validationReturnInfo.comodityAutoSn[i].message"
              :is-error="validationReturnInfo.comodityAutoSn[i].isError"
              @selectOptionClick="handleComodityClick($event, i)"
            />
          </div>

          <div class="form-box m_t_20">
            <comp-input
              :title="$t('ML60519')"
              :placeholder="$t('ML60520')"
              :message="validationReturnInfo.prdtBrandNm[i].message"
              :is-error="validationReturnInfo.prdtBrandNm[i].isError"
              @input="handlePrdtBrand($event, i)"
            />
          </div>

          <div class="form-box m_t_20">
            <comp-input
              :title="$t('ML60521')"
              :placeholder="$t('ML60522')"
              :message="validationReturnInfo.prdtNm[i].message"
              :is-error="validationReturnInfo.prdtNm[i].isError"
              @input="handlePrdt($event, i)"
            />
          </div>
          <div class="form-box m_t_20">
            <label>{{ $t('ML50275') }}</label>
            <div class="multiForm type-input-select">
              <div class="multi-item row1">
                <comp-input
                  id="price"
                  type="number"
                  :max="8"
                  :message="validationReturnInfo.prdtAmt[i].message"
                  :is-error="validationReturnInfo.prdtAmt[i].isError"
                  @input="handlePrdtAmt($event, i)"
                />
              </div>
              <div class="multi-item row2">
                <comp-select
                  id="price-unit"
                  :value="curUnitCdAll.curUnitCd"
                  :message="validationReturnInfo.curUnitCd[i].message"
                  :is-error="validationReturnInfo.curUnitCd[i].isError"
                  :option-list="curUnitCdList"
                  @selectOptionClick="handlePrdtAmtClick($event)"
                />
              </div>
            </div>
          </div>
          <comp-counter
            :id="i"
            v-model="reqInfo[i].prdtCnt"
            class-name="m_t_20"
            :label="$t('ML50276')"
            :max="2"
            @update:count="handleItemCountChange($event, i)"
          />
        </div>
        <comp-accordion
          v-if="!isFast"
          wrapper-class="type-delivery-benefit m_t_32"
        >
          <template #accordion-trigger>
            {{ $t('ML50768') }} {{ $t('ML50769') }}
          </template>
          <template #accordion-content>
            <comp-input
              id="item-order-number"
              :title="$t('ML50770')"
              :value="reqInfo[i].shopmallOrdNoInfo"
              :placeholder="$t('ML50771')"
              @input="handleShopmallOrdNoInfo($event, i)"
            />
            <comp-input
              id="item-color"
              class-name="m_t_20"
              :title="$t('ML50772')"
              :value="reqInfo[i].prdtColorInfo"
              :placeholder="$t('ML50773')"
              @input="handlePrdtColorInfo($event, i)"
            />
            <comp-input
              id="item-size"
              class-name="m_t_20"
              :title="$t('ML50774')"
              :value="reqInfo[i].prdtSizeInfo"
              :placeholder="$t('ML50775')"
              @input="handlePrdtSizeInfo($event, i)"
            />
            <comp-input
              id="item-url"
              class-name="m_t_20"
              :title="$t('ML50776')"
              :value="reqInfo[i].prdtDtlUrl"
              :placeholder="$t('ML50777')"
              @input="handlePrdtDtlUrl($event, i)"
            />
            <div class="form-box m_t_20">
              <label>{{ $t('ML50314') }}</label>
              <comp-image-loader
                :use-search="true"
                :use-select="false"
                :label="$t('ML50316')"
                :show-allow-types-message="false"
              />
            </div>
          </template>
        </comp-accordion>
        <div class="box_wrap type-product-payment m_t_32">
          <dl class="sum">
            <dt>{{ $t('ML60523') }}</dt>
            <dd class="">
              {{ reqInfo[i].itemPrice }} <em>{{ curUnitCdAll.curUnitCd }}</em>
              <span>({{ 0 }} USD)</span>
            </dd>
          </dl>
        </div>
      </div>
    </div>
  </div>

  <div class="lp_foot">
    <div class="layout">
      <comp-button-wrap
        class-name="grid-row bs-8 type-full"
        button-size-class="type-48"
      >
        <comp-button
          button-style-class="type2"
          :name="$t('ML60524')"
          :disabled="disabled"
          @btnClick="handleAddItemClick"
        />
        <comp-button
          button-style-class="type1"
          :name="$t('ML60525')"
          :disabled="disabled"
          @btnClick="reqPrdt"
        />
      </comp-button-wrap>
    </div>
  </div>
  {{ comodityAutoSnArray.length }} <br>
  {{ buyerInfo }}<br>
  {{ reqInfo }}
</template>

<script>
import { useStore } from 'vuex'
import CompButtonWrap from '@comp/common/ButtonWrap'
import CompButton from '@comp/common/Button'
import CompInput from '@comp/common/Input/Input'
import CompSelect from '@comp/common/Input/Select.vue'
import CompAccordion from '@comp/common/Accordion'
import CompCounter from '@comp/common/Input/Counter'
import CompImageLoader from '@comp/common/ImageLoader'
import { computed } from '@vue/runtime-core'
import { ref, getCurrentInstance, reactive } from 'vue'
import { isLogin } from '@utils/login/loginUtil'
import { API_RESULT_STATUS } from '@constants/common_constant'
import { getComodityList } from '@api/delivery/deliveryApi'
import { tsParameterProperty } from '@babel/types'
import { reqMyHootPrdt } from '@api/delivery/deliveryRequestApi'

export default {
  components: {
    CompButtonWrap,
    CompButton,
    CompInput,
    CompSelect,
    CompAccordion,
    CompCounter,
    CompImageLoader
  },
  setup() {
    const store = useStore()
    const userInfo = computed(() => store.state.memberStore.userInfo) || ''
    const passedSlideOptions = store.state.temp[store.state.temp.length - 1]?.options
    const globalFnc = getCurrentInstance().appContext.config.globalProperties

    // isError노출 = true (픽업 예약 버튼 클릭시 true로 바뀌면서 isError 활성화 조건으로 사용)
    const validation = ref(false)
    const shopMallUseYn = ref({ yn: false })
    // 알림 메시지
    const SUCCESS_MSG = `주문번호 [${passedSlideOptions?.reqNo}] 의 배송신청이 완료되었습니다. `
    const FAIL_MSG = '배송신청이 완료되지 못했습니다.'
    const VALIDATION_MSG = `${FAIL_MSG} 필수값을 입력(선택)해주세요`
    const VALIDATION_SELECT_OPTION_MSG = '선택해주세요'
    const VALIDATION_INPUT_MSG = '입력해주세요'

    // 세팅 값
    const buyerInfo = ref({
      reqNo: '202211160445073900', // (주문번호) 이전페이지에서 넘겨준 값을 받아야 함 passedSlideOptions?.reqNo
      custPoBoxNo: 'P4C522', // userInfo.value.poBoxNo
      shopmallUrlAutoSn: '',
      shopmallUrl: '',

      centerAutoSn: '1000', // (센터값) 이전페이지에서 넘겨준 값을 받아야 함 passedSlideOptions?.centerAutoSn
      reqDate: '2023.02.07', // (주문일자) 이전페이지에서 넘겨준 값을 받아야 함 passedSlideOptions?.reqDate
      flag: store.state.memberStore.userInfo.custCountryCd.toLowerCase(),
      custNm: store.state.memberStore.userInfo.nickname
    })

    // 통화 단위 (모든 상품 공통)
    const curUnitCdAll = ref({ curUnitCd: '' })

    // 각 상품별 품목 (상품개수 = 품목개수)
    const comodityList = ref([{ id: '', title: '품목선택' }])
    const comodityAutoSnArray = ref([
      [{ id: '', title: '품목선택' }]
    ])
    // 배송신청 Info
    const reqInfo = ref([{
      hdsAutoSn: '',
      dlvTrackNo: '',
      comodityCatAutoSn: '',
      comodityAutoSn: '',
      prdtBrandNm: '',
      prdtNm: '',
      prdtAmt: '',
      curUnitCd: '',
      prdtCnt: 0,
      shopmallOrdNoInfo: '',
      prdtColorInfo: '',
      prdtSizeInfo: '',
      prdtDtlUrl: '',
      itemPrice: 0
    }])



    // 카테고리 select option 리스트 호출값
    const categoryOptList = computed(() => store.getters['deliveryAgencyStore/getComodityCatAutoSn'])
    // 쇼핑몰 select option 리스트 호출값
    const shopMallOptList = computed(() => store.getters['deliveryAgencyStore/getShopMallUrlAutoSn'])
    // 운송사 select option 리스트 호출값
    const hdsAutoSnOptList = computed(() => store.getters['deliveryAgencyStore/getHdsAutoSn'])
    // 통화단위 select option 리스트 호출값
    const curUnitCdList = computed(() => store.getters['deliveryAgencyStore/getCurUnitcd'])



    // 카테고리 호출
    store.dispatch('deliveryAgencyStore/fetchComodityCatAutoSn')
    // 쇼핑몰 URL 호출
    store.dispatch('deliveryAgencyStore/fetchShopMallUrlAutoSn', { centerAutoSn: buyerInfo.value.centerAutoSn })
    // 운송사 호출
    store.dispatch('deliveryAgencyStore/fetchHdsAutoSn', { centerAutoSn: buyerInfo.value.centerAutoSn })
    // 통화단위 호출
    store.dispatch('deliveryAgencyStore/fetchCurUnitcd') // 통화단위 action


    // 쇼핑몰 URL 선택시
    const handleShoppingMallUrlClick = (data, itemNum) => {
      // 쇼핑몰 직접입력일 경우 readOnly:false
      data.id === '' ? shopMallUseYn.value.yn = false : shopMallUseYn.value.yn = true
      buyerInfo.value.shopmallUrlAutoSn = data.id
      buyerInfo.value.shopmallUrl = data.url
    }
    // 트래킹번호 선택시
    const handlehdsAutoSnClick = (data, itemNum) => {
      reqInfo.value[itemNum].hdsAutoSn = data.id
    }
    // 카테고리 선택시
    const handleCategoryClick = (data, itemNum) => {
      reqInfo.value[itemNum].comodityCatAutoSn = data.id
      // 선택한 카테고리에 대한 품목값 세팅
      comodityListApi({ id: data.id }, itemNum)
    }
    // 품목 선택시
    const handleComodityClick = (data, itemNum) => {
      reqInfo.value[itemNum].comodityAutoSn = data.id
    }
    // 단가 통화단위 선택시
    const handlePrdtAmtClick = (data) => {
      curUnitCdAll.value.curUnitCd = data.id
      for(const i in reqInfo.value) {
        reqInfo.value[i].curUnitCd = curUnitCdAll.value.curUnitCd
      }
    }



    // 쇼핑몰 URL input
    const handleShoppingMall = (data, itemNum) => {
      buyerInfo.value.shopmallUrl = data.target.value
    }
    // 트래킹번호 input
    const handleDlvTrackNo = (data, itemNum) => {
      reqInfo.value[itemNum].dlvTrackNo = data.target.value
    }
    // 브랜드명 input
    const handlePrdtBrand = (data, itemNum) => {
      reqInfo.value[itemNum].prdtBrandNm = data.target.value
    }
    // 상품명 input
    const handlePrdt = (data, itemNum) => {
      reqInfo.value[itemNum].prdtNm = data.target.value
    }
    // 단가 input
    const handlePrdtAmt = (data, itemNum) => {
      reqInfo.value[itemNum].prdtAmt = data.target.value
      totalPrice(itemNum)// (단가 X 수랑) 계산
    }
    // 수량
    const handleItemCountChange = (data, itemNum) => {
      reqInfo.value[itemNum].prdtCnt = data
      totalPrice(itemNum)// (단가 X 수랑) 계산
    }
    // 쇼핑몰주문번호정보
    const handleShopmallOrdNoInfo = (data, itemNum) => {
      reqInfo.value[itemNum].shopmallOrdNoInfo = data.target.value
    }
    // 상품색상정보
    const handlePrdtColorInfo = (data, itemNum) => {
      reqInfo.value[itemNum].prdtColorInfo = data.target.value
    }
    // 상품사이즈정보
    const handlePrdtSizeInfo = (data, itemNum) => {
      reqInfo.value[itemNum].prdtSizeInfo = data.target.value
    }
    // 상품상세URL
    const handlePrdtDtlUrl = (data, itemNum) => {
      reqInfo.value[itemNum].prdtDtlUrl = data.target.value
    }


    // 상품 수 객체
    const items = ref([{ id: 0, count: 0 }])

    // 상품 추가 버튼
    function handleAddItemClick() {
      console.log('상품추가 누름!')
      items.value = [...items.value, { id: items.value.length }]
      addItemAndComodityOjbect() // 상품, 품목의 객체 각각 배열에 추가
    }
    // 상품 삭제 버튼
    function handleItemDeleteClick(itemNum) {
      if (itemNum === 0) return
      items.value = items.value.filter((_, idx) => idx !== itemNum)
      deleteItemAndComodityOjbect(itemNum) // 해당 index의 상품객체, 품목객체 배열에서 삭제
    }



    // 객체 추가 함수
    function addItemAndComodityOjbect() {
      // 상품객체 추가
      reqInfo.value.push({
        hdsAutoSn: '',
        dlvTrackNo: '',
        comodityCatAutoSn: '',
        comodityAutoSn: '',
        prdtBrandNm: '',
        prdtNm: '',
        prdtAmt: '',
        curUnitCd: '',
        prdtCnt: 0,
        shopmallOrdNoInfo: '',
        prdtColorInfo: '',
        prdtSizeInfo: '',
        prdtDtlUrl: '',
        itemPrice: 0
      })
      // 추가된 상품의 품목 select option 생성
      comodityAutoSnArray.value.push([{ id: '', title: '품목선택' }])
    }

    // 객체 삭제 함수
    function deleteItemAndComodityOjbect(itemNum) {
      // index번째의 상품 객체 삭제
      reqInfo.value.splice(itemNum, 1)
      // index번째의 품목 객체 삭제
      comodityAutoSnArray.value.splice(itemNum, 1)
    }

    // 품목 리스트 API
    async function comodityListApi(payload, itemNum) {
      comodityList.value = [{ id: '', title: '품목선택' }]
      await getComodityList(payload)
        .then(res => {
          if (res.status === API_RESULT_STATUS.SUCCESS) {
            const dataList = res.data.dataList
            for (const i in dataList) {
              comodityList.value.push({
                id: dataList[i].comodityAutoSn,
                title: dataList[i].comodityNm
              })
            }
            console.log('품목: ', comodityList.value)
            comodityAutoSnArray.value[itemNum] = comodityList.value
          }
        })
    }

    // 배송신청
    async function reqPrdt() {
      validation.value = true // isError 노출 조건 true
      // 검증
      if(validationCheck()) {
        return alert(VALIDATION_MSG)
      }
      // req 데이터
      const payload = {
        custPoBoxNo: buyerInfo.value.custPoBoxNo,
        reqNo: buyerInfo.value.reqNo,
        shopmallUrlAutoSn: buyerInfo.value.shopmallUrlAutoSn,
        shopmallUrl: buyerInfo.value.shopmallUrl,
        prdtList: reqInfo.value
      }
      // 배송신청 API
      await reqMyHootPrdt(payload)
        .then(res => {
          if (res.status === API_RESULT_STATUS.SUCCESS) {
            console.log('배송완료 결과:', res)
            alert(SUCCESS_MSG)
          }
        }).catch((e) => {
          if(e.status === API_RESULT_STATUS.ERROR) {
            return alert(`${FAIL_MSG} ${e.data.message}`)
          }
        })
    }

    // 상품가 금액 계산 함수
    function totalPrice(itemNum) {
      reqInfo.value[itemNum].itemPrice = Number(reqInfo.value[itemNum].prdtAmt) * Number(reqInfo.value[itemNum].prdtCnt)
    }


    // validation 처리
    const validationReturnInfo = reactive({
      shopmallUrlAutoSn: computed(() => {
        if(globalFnc.$isNull(buyerInfo.value.shopmallUrlAutoSn) && validation.value) {
          return { message: '선택해 주세요.', isError: true }
        }
        return { message: '', isError: false }
      }),

      shopmallUrl: computed(() => {
        if(globalFnc.$isNull(buyerInfo.value.shopmallUrl) && validation.value) {
          return { message: '입력해 주세요.', isError: true }
        }
        return { message: '', isError: false }
      }),

      hdsAutoSn: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].hdsAutoSn, errorArr, VALIDATION_SELECT_OPTION_MSG)
        }
        return errorArr
      }),

      dlvTrackNo: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].dlvTrackNo, errorArr, VALIDATION_INPUT_MSG)
        }
        return errorArr
      }),

      comodityCatAutoSn: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].comodityCatAutoSn, errorArr, VALIDATION_SELECT_OPTION_MSG)
        }
        return errorArr
      }),

      comodityAutoSn: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].comodityAutoSn, errorArr, VALIDATION_SELECT_OPTION_MSG)
        }
        return errorArr
      }),

      prdtBrandNm: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].prdtBrandNm, errorArr, VALIDATION_INPUT_MSG)
        }
        return errorArr
      }),

      prdtNm: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].prdtNm, errorArr, VALIDATION_INPUT_MSG)
        }
        return errorArr
      }),

      prdtAmt: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].prdtAmt, errorArr, VALIDATION_INPUT_MSG)
        }
        return errorArr
      }),

      curUnitCd: computed(() => {
        let errorArr = []
        const infoArr = reqInfo.value
        for(const i in infoArr) {
          errorArr = errorValidationFunc(infoArr[i].curUnitCd, errorArr, VALIDATION_SELECT_OPTION_MSG)
        }
        return errorArr
      })


    })

    // 에러 메시지 함수
    function errorValidationFunc(value, errorArr, message) {
      if(globalFnc.$isNull(value) && validation.value) {
        errorArr.push({ message: message, isError: true })
      }else{
        errorArr.push({ message: '', isError: false })
      }
      return errorArr
    }

    // validation중 하나라도 isError = true 일시 true 리턴
    function validationCheck() {
      let result = false
      const strObj = validationReturnInfo

      const strArr = Object.keys(strObj).map(item => {
        // 만약 validationReturnInfo key 의 value가 배열이라면
        if(Array.isArray(strObj[item])) {
          for(const i in strObj[item]) {
            result = !!strObj[item][i].isError
            // console.log('>>>>result', result)
          }
          return result
        }
        return strObj[item].isError // 배열이 아니면 value.isError 리턴
      })
      console.log(strArr)
      if(strArr.includes(true)) {
        return true
      }
      return false
    }

    return{
      buyerInfo,
      reqInfo,
      shopMallUseYn,
      curUnitCdAll,
      comodityAutoSnArray,
      validationReturnInfo,

      categoryOptList,
      shopMallOptList,
      hdsAutoSnOptList,
      curUnitCdList,

      handleShoppingMallUrlClick,
      handlehdsAutoSnClick,
      handleCategoryClick,
      handleComodityClick,
      handlePrdtAmtClick,

      handleShoppingMall,
      handleDlvTrackNo,
      handlePrdtBrand,
      handlePrdt,
      handlePrdtAmt,
      handleItemCountChange,
      handleShopmallOrdNoInfo,
      handlePrdtColorInfo,
      handlePrdtSizeInfo,
      handlePrdtDtlUrl,

      handleAddItemClick,
      handleItemDeleteClick,
      items,
      reqPrdt
    }
  },
  mounted() {
    this.$updateSlideOptions({
      // 다국어: 중복
      title: 'PL00146'
    })
  }

}
</script>

<style lang="scss" scoped>
.type-product-payment {
  .sum {
    dt {
      line-height: 24px;
      font-size: 14px;
      color: #333;
    }
    dd {
      line-height: 24px;
      font-size: 20px;
      color: #333;
      > em {
        line-height: 24px;
        font-size: 15px;
        color: #333;
      }
    }
  }
}
.message {
  margin-top: 12px;
  line-height: 16px;
  img {
    display: inline-block;
    margin-right: 4px;
  }
}
</style>

```
