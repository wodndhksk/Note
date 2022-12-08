```javascript
setup() {
    const store = useStore()
    const prevPageSlideOptions = computed(() => { return store.state.temp[store.state.temp.length - 2]?.options })
    const prevPage = store.state.temp[store.state.temp.length - 1]
    const isEdit = !!prevPage?.options?.isEdit

    const stepInfo = reactive(computed(() => store.getters['deliveryAgencyReqStore/getReqInfo']))

    console.log('step5 초기값 ', stepInfo.value.insuranceJoinFee)
    // 포장서비스 - 브랜드박스 제거
    const setBrandBoxRemove = (value, checked) => {
      console.log('setBrandBoxRemove', checked)
      const tempInfo = {
        brandBoxRemove: checked
      }
      store.commit('deliveryAgencyReqStore/mutationReqInfo5', tempInfo)
    }
    // 포장서비스 - Invoice 제거
    const setInvoiceRemove = (value, checked) => {
      const tempInfo = {
        invoiceRemove: checked
      }
      store.commit('deliveryAgencyReqStore/mutationReqInfo5', tempInfo)
    }
    // 보험 서비스
    const setInsuranceJoinFee = (value) => {
      console.log('insurance', value)
      const tempInfo = {
        insuranceJoinFee: value
      }
      store.commit('deliveryAgencyReqStore/mutationReqInfo5', tempInfo)
    }

    // 트루패킹 서비스
    const setVideoPackReqYn = (value) => {
      console.log('videoPackReqYn', value)
      const tempInfo = {
        videoPackReqYn: value
      }
      store.commit('deliveryAgencyReqStore/mutationReqInfo5', tempInfo)
    }

    // 자동결제 신청 여부
    const setAutoPayReqYn = (value) => {
      console.log('autoPayReqYn', value)
      const tempInfo = {
        autoPayReqYn: value
      }
      store.commit('deliveryAgencyReqStore/mutationReqInfo5', tempInfo)
    }

    // 주의사항
    const setAgreePrecaution = (value, checked) => {
      console.log('agreePrecaution', checked)
      const tempInfo = {
        agreePrecaution: checked
      }
      store.commit('deliveryAgencyReqStore/mutationReqInfo5', tempInfo)
    }

    // 프로모션번호 (쿠폰번호)
    const setPromoNo = (value) => {
      console.log('promoNo', value)
      const tempInfo = {
        promoNo: value
      }
      store.commit('deliveryAgencyReqStore/mutationReqInfo5', tempInfo)
    }

    return {
      isEdit,
      prevPageSlideOptions,
      stepInfo,
      setBrandBoxRemove,
      setInvoiceRemove,
      setInsuranceJoinFee,
      setVideoPackReqYn,
      setAutoPayReqYn,
      setAgreePrecaution,
      setPromoNo
    }
  },
```
