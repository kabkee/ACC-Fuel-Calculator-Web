<template>
    <f7-page name="home">
        <!-- Top Navbar -->
        <f7-navbar :sliding="false">
            <!-- <f7-nav-left>
        <f7-link icon-ios="f7:menu" icon-aurora="f7:menu" icon-md="material:menu" panel-open="left"></f7-link>
      </f7-nav-left> -->
            <f7-nav-title sliding class="topTitle"
                ><span><GasStation /></span>ACC Fuel Calculator
                Web</f7-nav-title
            >
            <!-- <f7-nav-right>
        <f7-link icon-ios="f7:menu" icon-aurora="f7:menu" icon-md="material:menu" panel-open="right"></f7-link>
      </f7-nav-right>
      <f7-nav-title-large>ACC Fuel Calculator Web</f7-nav-title-large> -->
        </f7-navbar>

        <!-- Page content-->
        <f7-block style="margin-top: 0">
            <f7-row no-gap>
                <f7-col style="align-self: center; padding-top: 0.5em"
                    >Lap Time</f7-col
                >
                <f7-col class="laptime"
                    ><f7-list no-hairlines style="margin: 0">
                        <f7-list-input
                            label="Min"
                            floating-label
                            type="Number"
                            min="0"
                            max="999"
                            placeholder="Min"
                            clear-button
                            v-model:value="laptimeMin"
                        >
                        </f7-list-input>
                        <f7-list-input
                            label="Sec"
                            floating-label
                            type="Number"
                            min="0"
                            max="59"
                            placeholder="Sec"
                            clear-button
                            v-model:value="laptimeSec"
                        >
                        </f7-list-input> </f7-list
                ></f7-col>
            </f7-row>
            <f7-row no-gap>
                <f7-col style="align-self: center; padding-top: 0.5em"
                    >Stint Length</f7-col
                >
                <f7-col
                    ><f7-list no-hairlines style="margin: 0">
                        <f7-list-input
                            label="Min"
                            floating-label
                            type="Number"
                            min="0"
                            max="60"
                            style="width: 100%"
                            placeholder="Min"
                            clear-button
                            v-model:value="stintLength"
                        >
                        </f7-list-input></f7-list
                ></f7-col>
            </f7-row>
            <f7-row no-gap>
                <f7-col style="align-self: center; padding-top: 0.5em"
                    >Liters Per Lap</f7-col
                >
                <f7-col
                    ><f7-list no-hairlines style="margin: 0">
                        <f7-list-input
                            label="Liter"
                            floating-label
                            type="Number"
                            min="0"
                            max="60"
                            placeholder="Liter"
                            clear-button
                            step="0.1"
                            v-model:value="literPerLap"
                        >
                        </f7-list-input> </f7-list
                ></f7-col>
            </f7-row>
        </f7-block>
        <f7-row
            style="padding: 0 0.5em"
            v-if="(laptimeMin || laptimeSec) && stintLength && literPerLap"
        >
            <f7-col>
                <f7-card title="Risky" :content="`${riskyLiter} L`"></f7-card>
            </f7-col>
            <f7-col>
                <f7-card
                    title="Safe"
                    :content="`${safeLiter} L`"
                ></f7-card> </f7-col
        ></f7-row>
        <f7-row
            style="
                padding: 0 0.5em;
                text-align: center;
                color: #999;
                font-size: 1.2em;
            "
            v-else
        >
            <f7-col>
                <span>위에 빈칸을 채워주세요.</span>
            </f7-col>
        </f7-row>

        <!-- <f7-block-title>Navigation</f7-block-title>
        <f7-list>
            <f7-list-item link="/about/" title="About"></f7-list-item>
            <f7-list-item link="/form/" title="Form"></f7-list-item>
        </f7-list>

        <f7-block-title>Modals</f7-block-title>
        <f7-block strong>
            <f7-row no-gap>
                <f7-col width="50">
                    <f7-button fill raised popup-open="#my-popup"
                        >Popup</f7-button
                    >
                </f7-col>
                <f7-col width="50">
                    <f7-button fill raised login-screen-open="#my-login-screen"
                        >Login Screen</f7-button
                    >
                </f7-col>
            </f7-row>
        </f7-block> -->

        <!-- <f7-block-title>Panels</f7-block-title>
        <f7-block strong>
            <f7-row no-gap>
                <f7-col width="50">
                    <f7-button fill raised panel-open="left"
                        >Left Panel</f7-button
                    >
                </f7-col>
                <f7-col width="50">
                    <f7-button fill raised panel-open="right"
                        >Right Panel</f7-button
                    >
                </f7-col>
            </f7-row>
        </f7-block>

        <f7-list>
            <f7-list-item
                title="Dynamic (Component) Route"
                link="/dynamic-route/blog/45/post/125/?foo=bar#about"
            ></f7-list-item>
            <f7-list-item
                title="Default Route (404)"
                link="/load-something-that-doesnt-exist/"
            ></f7-list-item>
            <f7-list-item
                title="Request Data & Load"
                link="/request-and-load/user/123456/"
            ></f7-list-item>
        </f7-list> -->
    </f7-page>
</template>
<script>
import { GasStation } from "mdue";
import moemnt from "moment";

export default {
    components: {
        GasStation,
    },
    data() {
        return {
            laptime: {
                min: null,
                sec: null,
            },
            laptimeMin: null,
            laptimeSec: null,
            stintLength: null,
            literPerLap: null,
        };
    },
    computed: {
        laptimeToSec() {
            let secs =
                (this.laptimeMin ? parseInt(this.laptimeMin) * 60 : 0) +
                (this.laptimeSec ? parseInt(this.laptimeSec) : 0);

            return secs;
        },
        riskyLiter() {
            return String(
                parseInt(
                    ((this.stintLength * 60) / this.laptimeToSec) *
                        this.literPerLap
                )
            );
        },
        safeLiter() {
            return String(parseInt(this.riskyLiter * 1.05));
        },
    },
};
</script>

<style scoped>
.topTitle > span {
    vertical-align: middle;
    margin-right: 0.5em;
}
.col.laptime .list >>> ul {
    display: inline-flex;
}
.col .list >>> ul > li {
    min-width: calc(100% - 20px);
}
</style>
