## STORE

```javascript
const MODULE_NAME = 'deliveryAgencyReqStore'

const TYPES = {
  // state types
  CUST_PO_BOX_NO: 'custPoBoxNo',
  REQ_CENTER_AUTO_SN: 'reqCenterAutoSn',
  TRANS_MEANS_CD: 'transMeansCd',
  DLV_SVC_CD: 'dlvSvcCd',
  REQ_CAT_CD: 'reqCatCd',
  SHOP_MALL_URL_AUTOSN: 'shomallUrlAutoSn',
  SHOP_MALL_URL: 'shopmallUrl',
  FIX_DLV_REQ_YN: 'fixDlvReqYn',
  RECEIPT_REMOVE_REQ_YN: 'receiptRemoveReqYn',
  BOX_REMOVE_REQ_YN: 'boxRemoveReqYn',
  INSURANCE_REWARD_LIMIT_CD: 'insuranceRewardLimitCd',
  INSURANCE_JOIN_FEE: 'insuranceJoinFee', // 보험서비스 신청여부
  VIDEO_PACK_REQ_YN: 'videoPackReqYn', // 트루패킹 서비스
  PICKUP_ASK_YN: 'pickUpAskYn',
  PICKUP_ASK_CAT_CD: 'pickUpAskCatCd',
  AUTO_PAY_REQ_YN: 'autoPayReqYn', // 자동결제 신청여부
  PROMO_NO: 'promoNo', // 프로모션번호 (쿠폰번호)
  GIFT_REQ_YN: 'giftReqYn',
  DLV_PLACE_NM: 'dlvPlaceNm',
  DLV_COUNTRY_CD: 'dlvCountryCd',
  RCVR_NM: 'rcvrNm',
  COUNTRY_TEL_NO: 'countryTelNo',
  TEL_NO: 'telNo',
  ZIP: 'zip', // 우편번호
  COUNTRY_ADDR: 'countryAddr', // 주소
  DTL_ADDR: 'dtlAddr', // 상세주소
  BASE_ADDR: 'baseAddr',
  BASE_DLV_PLACE_YN: 'baseDlvPlaceYn',
  ADDR_ADD_YN: 'addrAddYn',
  DLV_ASK_INFO: 'dlvAskInfo', // 배송요청정보
  CLEARANCE_CHK_TYP_CD: 'clearanceChkTypCd',
  CLEARANCE_CHK_NO_NM: 'clearanceChkNoNm', // 통관확인번호명
  CLEARANCE_CHK_NO: 'clearanceChkNo', // 통관확인번호
  INFORM_COUNTRY_TEL_NO: 'informCountryTelNo', // 알림국가전화번호
  INFORM_TEL_NO: 'informTelNo', // 알림전화번호
  GIFT_SNDR_NM: 'giftSndrNm',
  GIFT_RCVR_NM: 'giftRcvrNm',
  GIFT_COUNTRY_TEL_NO: 'giftCountryTelNo',
  GIFT_TEL_NO: 'giftTelNo',
  GIFT_TRANSMIT_CNTENT: 'giftTransmitCntent',
  BRAND_BOX_REMOVE: 'brandBoxRemove', // Step5 (포장서비스) 브랜드박스 제거
  INVOICE_REMOVE: 'invoiceRemove', // Step5 (포장서비스) Invoice 제거
  AGREE_PRECAUTION: 'agreePrecaution', // Step5 주의사항 체크여부
  SAME_AS_TEL_NO_CHK: 'sameAsTelNoChk', // Step5 위 연락처와 동일 체크박스

  // GETTER_DOUBLE_COUNT: 'doubleCount',
  // getter types
  GETTER_REQ_INFO: 'getReqInfo',

  // MUTATION_PLUS_COUNT: 'plusCount',
  // mutations type
  MUTATION_INIT_REQ_INFO: 'mutationInitReqInfo',
  MUTATION_REQ_INFO: 'mutationReqInfo',
  MUTATION_REQ_STEP_2_INFO: 'mutationReqInfo2',
  MUTATION_REQ_STEP_3_INFO: 'mutationReqInfo3',
  MUTATION_REQ_STEP_5_INFO: 'mutationReqInfo5',
  MUTATION_REQ_STEP_6_INFO: 'mutationReqInfo6',

  // action type
  ACTIONS_REQ_INFO: 'fetchReqInfo'
  // ACTIONS_FETCH_COUNT: 'fetchCount'
}

const store = {
  namespaced: true,

  state: {
    [TYPES.CUST_PO_BOX_NO]: '',
    [TYPES.REQ_CENTER_AUTO_SN]: '',
    [TYPES.TRANS_MEANS_CD]: 'AIR',
    [TYPES.DLV_SVC_CD]: 'DLV_AGENCY',
    [TYPES.REQ_CAT_CD]: '',
    [TYPES.SHOP_MALL_URL_AUTOSN]: '',
    [TYPES.SHOP_MALL_URL]: '',
    [TYPES.FIX_DLV_REQ_YN]: '',
    [TYPES.RECEIPT_REMOVE_REQ_YN]: '',
    [TYPES.BOX_REMOVE_REQ_YN]: '',
    [TYPES.INSURANCE_REWARD_LIMIT_CD]: '',
    [TYPES.INSURANCE_JOIN_FEE]: { label: '미신청' },
    [TYPES.VIDEO_PACK_REQ_YN]: { label: '미신청' },
    [TYPES.PICKUP_ASK_YN]: 'N',
    [TYPES.PICKUP_ASK_CAT_CD]: '',
    [TYPES.AUTO_PAY_REQ_YN]: { label: '미신청' },
    [TYPES.PROMO_NO]: '',
    [TYPES.GIFT_REQ_YN]: '',
    [TYPES.DLV_PLACE_NM]: '',
    [TYPES.ZIP]: '',
    [TYPES.COUNTRY_ADDR]: '',
    [TYPES.DTL_ADDR]: '',
    [TYPES.BASE_ADDR]: '',
    [TYPES.BASE_DLV_PLACE_YN]: '',
    [TYPES.ADDR_ADD_YN]: '',
    [TYPES.DLV_ASK_INFO]: '',
    [TYPES.CLEARANCE_CHK_TYP_CD]: '',
    [TYPES.CLEARANCE_CHK_NO_NM]: '',
    [TYPES.CLEARANCE_CHK_NO]: '',
    [TYPES.INFORM_COUNTRY_TEL_NO]: '',
    [TYPES.INFORM_TEL_NO]: '',
    [TYPES.GIFT_SNDR_NM]: '',
    [TYPES.GIFT_RCVR_NM]: '',
    [TYPES.GIFT_COUNTRY_TEL_NO]: '',
    [TYPES.GIFT_TEL_NO]: '',
    [TYPES.GIFT_TRANSMIT_CNTENT]: '',
    [TYPES.BRAND_BOX_REMOVE]: false,
    [TYPES.INVOICE_REMOVE]: false,
    [TYPES.AGREE_PRECAUTION]: true,
    [TYPES.SAME_AS_TEL_NO_CHK]: false

  },

  mutations: {
    [TYPES.MUTATION_INIT_REQ_INFO](state) {
      state[TYPES.CUST_PO_BOX_NO] = ''
      state[TYPES.REQ_CENTER_AUTO_SN] = ''
      state[TYPES.TRANS_MEANS_CD] = 'AIR'
      state[TYPES.DLV_SVC_CD] = ''
      state[TYPES.REQ_CAT_CD] = 'DLV_AGENCY'
      state[TYPES.SHOP_MALL_URL_AUTOSN] = ''
      state[TYPES.SHOP_MALL_URL] = ''
      state[TYPES.FIX_DLV_REQ_YN] = ''
      state[TYPES.RECEIPT_REMOVE_REQ_YN] = ''
      state[TYPES.BOX_REMOVE_REQ_YN] = ''
      state[TYPES.INSURANCE_REWARD_LIMIT_CD] = ''
      state[TYPES.INSURANCE_JOIN_FEE] = ''
      state[TYPES.VIDEO_PACK_REQ_YN] = ''
      state[TYPES.PICKUP_ASK_YN] = 'N'
      state[TYPES.PICKUP_ASK_CAT_CD] = ''
      state[TYPES.AUTO_PAY_REQ_YN] = ''
      state[TYPES.PROMO_NO] = ''
      state[TYPES.GIFT_REQ_YN] = ''
      state[TYPES.DLV_PLACE_NM] = ''
      state[TYPES.ZIP] = ''
      state[TYPES.COUNTRY_ADDR] = ''
      state[TYPES.DTL_ADDR] = ''
      state[TYPES.BASE_ADDR] = ''
      state[TYPES.BASE_DLV_PLACE_YN] = ''
      state[TYPES.ADDR_ADD_YN] = ''
      state[TYPES.DLV_ASK_INFO] = ''
      state[TYPES.CLEARANCE_CHK_TYP_CD] = ''
      state[TYPES.CLEARANCE_CHK_NO_NM] = ''
      state[TYPES.CLEARANCE_CHK_NO] = ''
      state[TYPES.INFORM_COUNTRY_TEL_NO] = ''
      state[TYPES.INFORM_TEL_NO] = ''
      state[TYPES.GIFT_SNDR_NM] = ''
      state[TYPES.GIFT_RCVR_NM] = ''
      state[TYPES.GIFT_COUNTRY_TEL_NO] = ''
      state[TYPES.GIFT_TEL_NO] = ''
      state[TYPES.GIFT_TRANSMIT_CNTENT] = ''
      state[TYPES.BRAND_BOX_REMOVE] = false
      state[TYPES.INVOICE_REMOVE] = false
      state[TYPES.AGREE_PRECAUTION] = true
      state[TYPES.SAME_AS_TEL_NO_CHK] = false
    },
    [TYPES.MUTATION_REQ_INFO](state, reqInfo) {
      // 고객신청센터코드
      if(reqInfo.reqCenterAutoSn !== undefined) {
        state[TYPES.REQ_CENTER_AUTO_SN] = reqInfo.reqCenterAutoSn
      }

      // 운송수단코드(항공/해상)
      if(reqInfo.transMeansCd !== undefined) {
        state[TYPES.TRANS_MEANS_CD] = reqInfo.transMeansCd
      }

      // 집하요청여부
      if(reqInfo.pickUpAskYn !== undefined) {
        state[TYPES.PICKUP_ASK_YN] = reqInfo.pickUpAskYn
      }

      // 집하요청구분코드(USPS, 한진택배)
      if(reqInfo.pickUpAskCatCd !== undefined) {
        state[TYPES.PICKUP_ASK_CAT_CD] = reqInfo.pickUpAskCatCd
      }
    },
    // step 2
    [TYPES.MUTATION_REQ_STEP_2_INFO](state, reqInfo) {
      // 배송서비스코드(일반/더빠른)
      if(reqInfo.dlvSvcCd !== undefined) {
        state[TYPES.DLV_SVC_CD] = reqInfo.dlvSvcCd
      }
    },
    // step 3
    [TYPES.MUTATION_REQ_STEP_3_INFO](state, reqInfo) {
      // 쇼핑몰 번호
      if(reqInfo.shomallUrlAutoSn !== undefined) {
        state[TYPES.SHOP_MALL_URL_AUTOSN] = reqInfo.shomallUrlAutoSn
      }

      // 쇼핑몰 url
      if(reqInfo.shopmallUrl !== undefined) {
        state[TYPES.SHOP_MALL_URL] = reqInfo.shopmallUrl
      }
    },
    [TYPES.MUTATION_REQ_STEP_5_INFO](state, reqInfo) {
      // 브랜드박스제거 (포장서비스)
      if(reqInfo.brandBoxRemove !== undefined) {
        state[TYPES.BRAND_BOX_REMOVE] = reqInfo.brandBoxRemove
      }

      // Invoice제거 (포장서비스)
      if(reqInfo.invoiceRemove !== undefined) {
        state[TYPES.INVOICE_REMOVE] = reqInfo.invoiceRemove
      }

      // 보험서비스
      if(reqInfo.insuranceJoinFee !== undefined) {
        state[TYPES.INSURANCE_JOIN_FEE] = reqInfo.insuranceJoinFee
      }

      // 트루패킹 서비스
      if(reqInfo.videoPackReqYn !== undefined) {
        state[TYPES.VIDEO_PACK_REQ_YN] = reqInfo.videoPackReqYn
      }

      // 자동결제 신청 여부
      if(reqInfo.autoPayReqYn !== undefined) {
        state[TYPES.AUTO_PAY_REQ_YN] = reqInfo.autoPayReqYn
      }

      // 주의사항 체크
      if(reqInfo.agreePrecaution !== undefined) {
        state[TYPES.AGREE_PRECAUTION] = reqInfo.agreePrecaution
      }

      // 프로모션번호 (쿠폰번호)
      if(reqInfo.promoNo !== undefined) {
        state[TYPES.PROMO_NO] = reqInfo.promoNo
      }
    },
    // step 6
    [TYPES.MUTATION_REQ_STEP_6_INFO](state, reqInfo) {
      // 배송지명
      if(reqInfo.dlvPlaceNm !== undefined) {
        state[TYPES.DLV_PLACE_NM] = reqInfo.dlvPlaceNm
      }

      // 도착국가
      if(reqInfo.dlvSvcCd !== undefined) {
        state[TYPES.DLV_SVC_CD] = reqInfo.dlvSvcCd
      }

      // 수취인명
      if(reqInfo.rcvrNm !== undefined) {
        state[TYPES.RCVR_NM] = reqInfo.rcvrNm
      }

      // 국가전화번호
      if(reqInfo.countryTelNo !== undefined) {
        state[TYPES.COUNTRY_TEL_NO] = reqInfo.countryTelNo
      }

      // 전화번호
      if(reqInfo.telNo !== undefined) {
        state[TYPES.TEL_NO] = reqInfo.telNo
      }

      // 주소 (zip)
      if(reqInfo.zip !== undefined) {
        state[TYPES.ZIP] = reqInfo.zip
      }

      // 주소 (countryAddr)
      if(reqInfo.countryAddr !== undefined) {
        state[TYPES.COUNTRY_ADDR] = reqInfo.countryAddr
      }

      // 상세주소 (dtlAddr)
      if(reqInfo.dtlAddr !== undefined) {
        state[TYPES.DTL_ADDR] = reqInfo.dtlAddr
      }

      // 배송요청사항
      if(reqInfo.dlvAskInfo !== undefined) {
        state[TYPES.DLV_ASK_INFO] = reqInfo.dlvAskInfo
      }

      // 통관확인번호 명
      if(reqInfo.clearanceChkNoNm !== undefined) {
        console.log(reqInfo.clearanceChkNoNm)
        state[TYPES.CLEARANCE_CHK_NO_NM] = reqInfo.clearanceChkNoNm
      }

      // 통관확인번호
      if(reqInfo.clearanceChkNo !== undefined) {
        console.log(reqInfo.clearanceChkNo)
        state[TYPES.CLEARANCE_CHK_NO] = reqInfo.clearanceChkNo
      }

      // 알림국가전화번호
      if(reqInfo.informCountryTelNo !== undefined) {
        console.log(reqInfo.informCountryTelNo)
        state[TYPES.INFORM_COUNTRY_TEL_NO] = reqInfo.informCountryTelNo
      }

      // 알림전화번호
      if(reqInfo.informTelNo !== undefined) {
        console.log(reqInfo.informTelNo)
        state[TYPES.INFORM_TEL_NO] = reqInfo.informTelNo
      }

      // Step5 위 연락처와 동일 체크박스
      if(reqInfo.sameAsTelNoChk !== undefined) {
        console.log(reqInfo.sameAsTelNoChk)
        state[TYPES.SAME_AS_TEL_NO_CHK] = reqInfo.sameAsTelNoChk
        if(reqInfo.sameAsTelNoChk === true) {
          state[TYPES.INFORM_TEL_NO] = state[TYPES.TEL_NO]
          state[TYPES.INFORM_COUNTRY_TEL_NO] = { id: state[TYPES.COUNTRY_TEL_NO].id }
        }
      }
    }
  },

  actions: {
    [TYPES.ACTIONS_REQ_INFO]({ state, commit }, { newReqCenterAutoSn }) {
      commit(TYPES.ACTIONS_REQ_INFO, newReqCenterAutoSn)
    }
  },

  getters: {
    [TYPES.GETTER_REQ_INFO](state) {
      return state
    }
  }
}

export {
  MODULE_NAME,
  TYPES,
  store
}

```
