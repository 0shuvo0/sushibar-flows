<template>
    <button @click="showModal = true" class="btn btn-block btn-secondary">Split the bill</button>
    <div class="modal-wrapper" v-if="showModal">
        <div class="modal-content add-item-modal-conetnt">
            <i @click="showModal = false" class="bi bi-x modal-close-btn"></i>
            <p class="text-center title-text mt-2">Split the bill in {{ splitCount }}?</p>
            
            <div class="mt-2">
                <div class="cart-service-charge bb-0">
                    <span class="prop">Table total</span>
                    <span class="val">£{{ toPay }}</span>
                </div>
                <div class="cart-service-charge">
                    <span class="prop">For you</span>
                    <span class="val">£{{ amountForIndividual }}</span>
                </div>
                <div class="cart-service-charge cart-subtotal">
                    <span class="prop">Subtotal</span>
                    <span class="val">£{{ amountForIndividual }}</span>
                </div>
            </div>
            <PaySplittedBtn
             :subtotal="amountForIndividual"
             :spliCount="splitCount"
            />
            <div class="or-indicator my-1">
                <div class="hr"></div>
                <span>OR</span>
                <div class="hr"></div>
            </div>
            <button @click="showModal = false" class="btn btn-block btn-secondary">Pay another way</button>
        </div>
    </div> 
</template>

<script>
import PaySplittedBtn from './PaySplittedBtn.vue'

const SERVICE_CHARGE = 12.5

export default {
    components: {
        PaySplittedBtn
    },
    data(){
        return {
            showModal: true,
            splitCount: 2
        }
    },
    computed: {
        toPay(){
            return (+this.total + +this.serviceCharge).toFixed(2)
        },
        cart(){
            return this.$store.state.cart
        },
        total(){
            const total = this.cart.map(item => +item.totalPrice)
                                    .reduce((acc, val) => acc + val, 0)
            return +total
        },
        amountForIndividual(){
            return ((+this.total + +this.serviceCharge) / this.splitCount).toFixed(2)
        },
        serviceCharge(){
            return (this.total * (SERVICE_CHARGE / 100)).toFixed(2)
        }
    },
}
</script>

<style lang="scss">
.split-ctrl{
    display: flex;
    align-items: center;
    justify-content: center;
    user-select: none;
    gap: 2em;
    .ctrl{
        --size: 56px;
        width: var(--size);
        height: var(--size);
        border-radius: 50%;
        background-color: #D0D8DF;
        color: #788DA1;
        display: flex;
        align-items: center;
        cursor: pointer;
        justify-content: center;
        i{
            font-size: 2em;
        }
    }
    .count{
        font-weight: 500;
        font-size: 64px;
        letter-spacing: -0.01em;
        color: var(--color-dark);
    }
}
.splitted-amount{
    font-weight: 700;
    font-size: 18px;
    color: var(--color-dark);
    text-align: center;
    margin-top: .5em;
}
</style>

