//! انتخاب ساعت و تاریخ

<template>
  <q-field readonly outlined dense class="q-pa-none">
    <template #control>
      <div class="no-outline full-width" style="direction: rtl">
        {{ displayValue }}
      </div>
    </template>
    <template #prepend>
      <q-icon name="event" class="cursor-pointer">
        <q-popup-proxy ref="qDateProxy" :offset="[10, 10]">
          <div
            v-if="dateType === 'hourly' || dateType === 'daily'"
            style="width: 100%; height: 100%"
          >
            <div class="row">
              <div style="width: 150px">
                <q-btn
                  :label="$t('today')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(0, 0, 0, 'امروز')"
                />
                <q-btn
                  :label="$t('yesterday')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(1, 1, 0, 'دیروز')"
                />
                <q-btn
                  :label="$t('the_last_three_days')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(3, 0, 0, 'سه روز گذشته')"
                />
                <q-btn
                  :label="$t('the_last_seven_days')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(7, 0, 0, 'هفت روز گذشته')"
                />
                <q-btn
                  :label="$t('six_oclock')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(0, 0, 6, '6 ساعت')"
                />
                <q-btn
                  :label="$t('twelve_oclock')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(0, 0, 12, '12 ساعت')"
                />
                <q-btn
                  :label="$t('twenty_four_oclock')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(0, 0, 24, '24 ساعت')"
                />
              </div>
              <div>
                <q-date
                  ref="hourlyView"
                  v-model="date_range"
                  landscape
                  range
                  calendar="persian"
                />
              </div>
              <div class="q-pa-sm">
                <div class="col-6">
                  <q-input
                    v-model="start_time"
                    filled
                    type="time"
                    style="direction: ltr"
                  />
                </div>
                <div class="col-6">
                  <q-input
                    v-model="end_time"
                    filled
                    type="time"
                    style="direction: ltr"
                  />
                </div>
                <q-btn
                  :label="$t('confirm')"
                  class="full-width"
                  stretch
                  color="primary"
                  @click="updateDisp"
                />
              </div>
            </div>
          </div>
          <div
            v-else-if="dateType === 'monthly'"
            style="width: 100%; height: 100%"
          >
            <div class="row">
              <div style="width: 150px">
                <q-btn
                  :label="$t('this_month')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(0, 0, 0, 'امروز')"
                />
                <q-btn
                  :label="$t('last_month')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(1, 1, 0, 'دیروز')"
                />
                <q-btn
                  :label="$t('last_two_months')"
                  class="full-width"
                  stretch
                  flat
                  color="primary"
                  @click="updateDate(3, 0, 0, 'سه روز گذشته')"
                />
                <!-- <q-btn
                  :label="$t('confirm')"
                  class="full-width"
                  stretch
                  color="primary"
                  @click="updateDisp"
                /> -->
              </div>
              <q-card>
                <q-card-section class="column">
                  <div class="col-4">
                    <q-badge
                      :color="
                        with_range && ranged === 'from' ? 'green' : 'grey'
                      "
                      text-color="black"
                      :label="customFromDate"
                    />
                    <q-input
                      v-model="from"
                      class="col-4"
                      square
                      style="width: 80px"
                      :disable="!with_range"
                      outlined
                      dense
                      label="from"
                    />
                  </div>
                  <div class="col-4">
                    <q-badge
                      :color="with_range && ranged === 'to' ? 'green' : 'grey'"
                      text-color="black"
                      :label="customToDate"
                    />
                    <q-input
                      v-model="to"
                      :disable="!with_range"
                      class="col-4"
                      square
                      style="width: 80px"
                      outlined
                      dense
                      label="to"
                    />
                  </div>
                </q-card-section>
                <q-toggle v-model="with_range" label="Ranged" />
              </q-card>
              <q-card
                v-if="!with_range"
                class="my-custom-card q-ma-sm q-pa-none"
                flat
              >
                <q-card-section
                  class="q-ma-none q-pa-none"
                  style="width: inherit; height: inherit"
                >
                  <q-list
                    class="column q-ml-none q-pa-none q-ma-sm"
                    style="width: inherit; height: inherit"
                  >
                    <q-item
                      v-for="month in $i18n.locale == 'en-US'
                        ? months.gregory
                        : months.jalali"
                      :key="month.id"
                      clickable
                      :style="
                        month.active
                          ? 'height: 40px; border: 2px dashed #4CAF50; border-radius: 10px !important'
                          : 'height: 40px'
                      "
                      :class="
                        month.active
                          ? 'col-2 q-ma-sm q-pa-none text-blue'
                          : 'col-2 q-ma-sm q-pa-none text-black'
                      "
                      @click="updateActiveMonth(month.id)"
                    >
                      <q-item-section class="content-center">{{
                        month.id
                      }}</q-item-section>
                    </q-item>
                  </q-list>
                </q-card-section>
              </q-card>
              <q-tab-panels
                v-else-if="with_range"
                v-model="ranged"
                animated
                style="overflow: hidden"
                class="my-custom-card q-ma-sm q-pa-none"
              >
                <q-tab-panel
                  style="overflow: hidden"
                  class="q-ma-none q-pa-none"
                  name="from"
                >
                  <q-card-section
                    class="q-ma-none q-pa-none"
                    style="width: inherit; height: inherit"
                  >
                    <q-list
                      class="column q-ml-none q-pa-none q-ma-sm"
                      style="width: inherit; height: inherit"
                    >
                      <q-item
                        v-for="month in $i18n.locale == 'en-US'
                          ? ranged_months.gregory.from
                          : ranged_months.jalali.from"
                        :key="month.id"
                        :disable="isFromDateDisabled"
                        clickable
                        :style="
                          month.active
                            ? 'height: 40px; border: 2px dashed #4CAF50; border-radius: 10px !important'
                            : 'height: 40px'
                        "
                        :class="
                          month.active
                            ? 'col-2 q-ma-sm q-pa-none text-blue'
                            : 'col-2 q-ma-sm q-pa-none text-black'
                        "
                        @click="updateActiveRangedMonth(month.id)"
                      >
                        <q-item-section class="content-center">{{
                          month.id
                        }}</q-item-section>
                      </q-item>
                    </q-list>
                  </q-card-section>
                </q-tab-panel>
                <q-tab-panel
                  style="overflow: hidden"
                  class="q-ma-none q-pa-none"
                  name="to"
                >
                  <q-card-section
                    class="q-ma-none q-pa-none"
                    style="width: inherit; height: inherit"
                  >
                    <q-list
                      class="column q-ml-none q-pa-none q-ma-sm"
                      style="width: inherit; height: inherit"
                    >
                      <q-item
                        v-for="month in $i18n.locale == 'en-US'
                          ? ranged_months.gregory.to
                          : ranged_months.jalali.to"
                        :key="month.id"
                        :disable="isToDateDisabled"
                        clickable
                        :style="
                          month.active
                            ? 'height: 40px; border: 2px dashed #4CAF50; border-radius: 10px !important'
                            : 'height: 40px'
                        "
                        :class="
                          month.active
                            ? 'col-2 q-ma-sm q-pa-none text-blue'
                            : 'col-2 q-ma-sm q-pa-none text-black'
                        "
                        @click="updateActiveRangedMonth(month.id)"
                      >
                        <q-item-section class="content-center">{{
                          month.id
                        }}</q-item-section>
                      </q-item>
                    </q-list>
                  </q-card-section>
                </q-tab-panel>
              </q-tab-panels>
              <div class="q-pa-sm">
                <q-btn
                  :label="$t('confirm')"
                  class="full-width"
                  stretch
                  color="primary"
                  @click="updateDisplayedMonth"
                />
              </div>
            </div>
          </div>
        </q-popup-proxy>
      </q-icon>
    </template>
  </q-field>
</template>

<script>
import _ from "lodash";

import {
  dispRange,
  getGregorianDate,
  getNow,
  selectRange
} from "src/helpers/PersianDate";
export default {
  props: {
    dateMask: {
      default: "DD/MM/YYYY",
      type: String
    },
    dateOnly: {
      type: Boolean
    },
    dateType: {
      default: "hourly",
      type: String
    },
    dateOptions: {
      default: () => ({}),
      type: Object
    },
    timeMask: {
      default: "HH:mm",
      type: String
    },
    timeOnly: {
      type: Boolean
    },
    timeOptions: {
      default: () => ({}),
      type: Object
    },
    value: {
      default: null,
      type: Object
    }
  },
  data() {
    return {
      isFromDateDisabled: false,
      isToDateDisabled: false,
      currentValue: this.value,
      displayValue: null,
      lastValue: "",
      customFromDate: null,
      customToDate: null,
      ranged: "from",
      from: "1401",
      to: "1401",
      date_text: null,
      with_range: false,
      date_range: null,
      start_time: "00:00",
      end_time: "23:59",
      activeMonths: [],
      activeMonth: null,
      months: {
        gregory: [
          { id: 1, active: false, value: "Farvardin" },
          { id: 2, active: false, value: "Ordibehesht" },
          { id: 3, active: false, value: "Khordaad" },
          { id: 4, active: false, value: "Tir" },
          { id: 5, active: false, value: "Mordaad" },
          { id: 6, active: false, value: "Shahrivar" },
          { id: 7, active: false, value: "Mehr" },
          { id: 8, active: false, value: "Abaan" },
          { id: 9, active: false, value: "Azar" },
          { id: 10, active: false, value: "Dey" },
          { id: 11, active: false, value: "Bahman" },
          { id: 12, active: false, value: "Esfand" }
        ],
        jalali: [
          { id: 1, active: false, value: "فروردین" },
          { id: 2, active: false, value: "اردیبهشت" },
          { id: 3, active: false, value: "خرداد" },
          { id: 4, active: false, value: "تیر" },
          { id: 5, active: false, value: "مرداد" },
          { id: 6, active: false, value: "شهریور" },
          { id: 7, active: false, value: "مهر" },
          { id: 8, active: false, value: "آبان" },
          { id: 9, active: false, value: "آذر" },
          { id: 10, active: false, value: "دی" },
          { id: 11, active: false, value: "بهمن" },
          { id: 12, active: false, value: "اسفند" }
        ]
      },
      ranged_months: {
        gregory: {
          from: [
            { id: 1, active: false, value: "Farvardin" },
            { id: 2, active: false, value: "Ordibehesht" },
            { id: 3, active: false, value: "Khordaad" },
            { id: 4, active: false, value: "Tir" },
            { id: 5, active: false, value: "Mordaad" },
            { id: 6, active: false, value: "Shahrivar" },
            { id: 7, active: false, value: "Mehr" },
            { id: 8, active: false, value: "Abaan" },
            { id: 9, active: false, value: "Azar" },
            { id: 10, active: false, value: "Dey" },
            { id: 11, active: false, value: "Bahman" },
            { id: 12, active: false, value: "Esfand" }
          ],
          to: [
            { id: 1, active: false, value: "Farvardin" },
            { id: 2, active: false, value: "Ordibehesht" },
            { id: 3, active: false, value: "Khordaad" },
            { id: 4, active: false, value: "Tir" },
            { id: 5, active: false, value: "Mordaad" },
            { id: 6, active: false, value: "Shahrivar" },
            { id: 7, active: false, value: "Mehr" },
            { id: 8, active: false, value: "Abaan" },
            { id: 9, active: false, value: "Azar" },
            { id: 10, active: false, value: "Dey" },
            { id: 11, active: false, value: "Bahman" },
            { id: 12, active: false, value: "Esfand" }
          ]
        },
        jalali: {
          from: [
            { id: 1, active: false, value: "فروردین" },
            { id: 2, active: false, value: "اردیبهشت" },
            { id: 3, active: false, value: "خرداد" },
            { id: 4, active: false, value: "تیر" },
            { id: 5, active: false, value: "مرداد" },
            { id: 6, active: false, value: "شهریور" },
            { id: 7, active: false, value: "مهر" },
            { id: 8, active: false, value: "آبان" },
            { id: 9, active: false, value: "آذر" },
            { id: 10, active: false, value: "دی" },
            { id: 11, active: false, value: "بهمن" },
            { id: 12, active: false, value: "اسفند" }
          ],
          to: [
            { id: 1, active: false, value: "فروردین" },
            { id: 2, active: false, value: "اردیبهشت" },
            { id: 3, active: false, value: "خرداد" },
            { id: 4, active: false, value: "تیر" },
            { id: 5, active: false, value: "مرداد" },
            { id: 6, active: false, value: "شهریور" },
            { id: 7, active: false, value: "مهر" },
            { id: 8, active: false, value: "آبان" },
            { id: 9, active: false, value: "آذر" },
            { id: 10, active: false, value: "دی" },
            { id: 11, active: false, value: "بهمن" },
            { id: 12, active: false, value: "اسفند" }
          ]
        }
      },
      current_month: null
    };
  },
  computed: {
    attributes() {
      const { ...attributes } = this.$attrs;
      return {
        ...attributes,
        mask: this.mask,
        value: this.currentValue
      };
    },
    inputElement() {
      return this.$refs.input;
    },
    mask() {
      return this.maskDate.replace(/\w/g, "#");
    },
    maskDate() {
      const mask = [];
      if (!this.timeOnly) {
        mask.push(this.dateMask);
      }
      if (!this.dateOnly) {
        mask.push(this.timeMask);
      }
      return mask.join(" ");
    }
  },
  watch: {
    dateType: {
      immediate: true,
      handler(val, oldVal) {
        // do your code
        setTimeout(() => {
          // this.$refs?.monthlyView?.setView("Months");
          if (val === "monthly")
            this.$i18n.locale == "en-US"
              ? (this.displayValue =
                  this.months["gregory"][
                    parseInt(this.current_month) - 1
                  ].value)
              : (this.displayValue =
                  this.months["jalali"][
                    parseInt(this.current_month) - 1
                  ].value);
        }, 500);
      }
    }
  },
  created() {
    this.updateDate(0, 0, 0, "امروز");
    // this.currentValue = this.toMask(this.value);
  },
  mounted() {
    this.$emit("mounted", this);
    this.current_month = getNow().split(" ")[0].split("/")[1];
    this.$i18n.locale == "en-US"
      ? (this.months["gregory"][parseInt(this.current_month) - 1].active = true)
      : (this.months["jalali"][parseInt(this.current_month) - 1].active = true);
    this.activeMonths.push(parseInt(this.current_month));
  },
  methods: {
    updateDate(st_from, st_to, past_hour, alt_disp = null) {
      let resu = selectRange(st_from, st_to, past_hour, true);
      let date_rg = {
        from: resu.from,
        to: resu.to
      };
      this.date_range = date_rg.from === date_rg.to ? date_rg.from : date_rg;
      //  if (st_from == st_to) {
      //   this.start_time = "00:00";
      //   this.end_time = "23:59";
      // }
      this.start_time = resu.from_time;
      this.end_time = resu.to_time;
      if (alt_disp.includes("روز")) {
        this.start_time = "00:00";
        this.end_time = "23:59";
      } else {
        this.start_time = resu.from_time;
        this.end_time = resu.to_time;
      }
      this.updateDisp();
    },
    updateDisp() {
      this.currentValue = dispRange(
        this.date_range,
        this.start_time,
        this.end_time
      );

      this.displayValue = this.currentValue.disply_date;
      if (this.$refs.qDateProxy !== undefined) this.$refs.qDateProxy.hide();
      this.$emit("update:modelValue", this.currentValue);
    },
    updateActiveMonth(month) {
      // month.active = true;
      const selected_month_index =
        this.$i18n.locale == "en-US"
          ? this.months["gregory"].findIndex((moon) => moon.id === month)
          : this.months["jalali"].findIndex((moon) => moon.id === month);

      this.$i18n.locale == "en-US"
        ? this.months["gregory"][selected_month_index].active
          ? (this.months["gregory"][selected_month_index].active = false)
          : (this.months["gregory"][selected_month_index].active = true)
        : this.months["jalali"][selected_month_index].active
        ? (this.months["jalali"][selected_month_index].active = false)
        : (this.months["jalali"][selected_month_index].active = true);
      const active_moon_index = this.activeMonths.findIndex(
        (activeMoon) => activeMoon === month
      );

      active_moon_index === -1
        ? this.activeMonths.push(month)
        : this.activeMonths.splice(active_moon_index, 1);
    },
    updateActiveRangedMonth(month) {
      // this.ranged = "to";
      if (this.ranged === "from") {
        const selected_month_index_from =
          this.$i18n.locale == "en-US"
            ? this.ranged_months["gregory"]["from"].findIndex(
                (moon) => moon.id === month
              )
            : this.ranged_months["jalali"]["from"].findIndex(
                (moon) => moon.id === month
              );

        this.$i18n.locale == "en-US"
          ? this.ranged_months["gregory"]["from"][selected_month_index_from]
              .active
            ? (this.ranged_months["gregory"]["from"][
                selected_month_index_from
              ].active = false)
            : (this.ranged_months["gregory"]["from"][
                selected_month_index_from
              ].active = true)
          : this.ranged_months["jalali"]["from"][selected_month_index_from]
              .active
          ? (this.ranged_months["jalali"]["from"][
              selected_month_index_from
            ].active = false)
          : (this.ranged_months["jalali"]["from"][
              selected_month_index_from
            ].active = true);
        this.computeDate("from", selected_month_index_from);
        this.isFromDateDisabled = true;
        this.ranged = "to";
      } else if (this.ranged === "to") {
        const selected_month_index_to =
          this.$i18n.locale == "en-US"
            ? this.ranged_months["gregory"]["to"].findIndex(
                (moon) => moon.id === month
              )
            : this.ranged_months["jalali"]["to"].findIndex(
                (moon) => moon.id === month
              );
        this.$i18n.locale == "en-US"
          ? this.ranged_months["gregory"]["to"][selected_month_index_to].active
            ? (this.ranged_months["gregory"]["to"][
                selected_month_index_to
              ].active = false)
            : (this.ranged_months["gregory"]["to"][
                selected_month_index_to
              ].active = true)
          : this.ranged_months["jalali"]["to"][selected_month_index_to].active
          ? (this.ranged_months["jalali"]["to"][
              selected_month_index_to
            ].active = false)
          : (this.ranged_months["jalali"]["to"][
              selected_month_index_to
            ].active = true);
        this.isToDateDisabled = true;
        this.computeDate("to", selected_month_index_to);
      }
    },
    computeDate(range, index) {
      let x =
        this.$i18n.locale == "en-US"
          ? this.ranged_months["gregory"][range][index].value
          : this.ranged_months["jalali"][range][index].value;
      if (x)
        range === "from"
          ? (this.customFromDate = this.$t(range) + " " + x)
          : (this.customToDate = this.$t(range) + " " + x);
      else this.customFromDate = this.customToDate = x;
    },
    updateDisplayedMonth() {
      this.displayValue = "";
      let actives = [];
      if (!this.with_range) {
        switch (this.$i18n.locale) {
          case "en-US": {
            this.months["gregory"]?.forEach((element) => {
              if (element.active) actives.push(element.value);
              else this.displayValue += "";
            });
            break;
          }
          case "fa-IR": {
            this.months["jalali"]?.forEach((element) => {
              if (element.active) actives.push(element.value);
              else this.displayValue += "";
            });
            break;
          }
        }
        this.displayValue = actives.join(" , ");
        if (this.$refs.qDateProxy !== undefined) this.$refs.qDateProxy.hide();
        this.activeMonths = _.sortBy(this.activeMonths);
        this.createDate(this.activeMonths);
      } else {
        let from_active_index = null;
        let to_active_index = null;
        this.$i18n.locale == "en-US"
          ? (from_active_index = this.ranged_months["gregory"][
              "from"
            ].findIndex((month) => month.active))
          : (from_active_index = this.ranged_months["jalali"]["from"].findIndex(
              (month) => month.active
            ));
        this.$i18n.locale == "en-US"
          ? (to_active_index = this.ranged_months["gregory"]["to"].findIndex(
              (month) => month.active
            ))
          : (to_active_index = this.ranged_months["jalali"]["to"].findIndex(
              (month) => month.active
            ));
        let date_ranged = { start_j: null, end_j: null };

        this.$i18n.locale == "en-US"
          ? (date_ranged.start_j = `${this.from}-${this.ranged_months[
              "gregory"
            ]["from"][from_active_index].id
              .toString()
              .padStart(2, "0")}-01 00:00:00`)
          : (date_ranged.start_j = `${this.from}-${this.ranged_months["jalali"][
              "from"
            ][from_active_index].id
              .toString()
              .padStart(2, "0")}-01 00:00:00`);

        this.$i18n.locale == "en-US"
          ? (date_ranged.end_j = `${this.to}-${this.ranged_months["gregory"][
              "to"
            ][to_active_index].id
              .toString()
              .padStart(2, "0")}-01 23:59:59`)
          : (date_ranged.end_j = `${this.to}-${this.ranged_months["jalali"][
              "to"
            ][to_active_index].id
              .toString()
              .padStart(2, "0")}-01 23:59:59`);
        date_ranged.start_g = getGregorianDate(
          date_ranged.start_j,
          "YYYY-MM-DD HH:mm:ss"
        );
        date_ranged.end_g = getGregorianDate(
          date_ranged.end_j,
          "YYYY-MM-DD HH:mm:ss"
        );
        this.$i18n.locale == "en-US"
          ? (this.displayValue = `${this.$t("from")} ${
              this.ranged_months["gregory"]["from"][from_active_index].value
            } ${this.from} ${this.$t("to")} ${
              this.ranged_months["gregory"]["to"][to_active_index].value
            } ${this.to}`)
          : (this.displayValue = `${this.$t("from")} ${
              this.ranged_months["jalali"]["from"][from_active_index].value
            } ${this.from} ${this.$t("to")} ${
              this.ranged_months["jalali"]["to"][to_active_index].value
            } ${this.to}`);
        if (this.$refs.qDateProxy !== undefined) this.$refs.qDateProxy.hide();
        this.$emit("update:modelValue", date_ranged);
      }
    },
    createDate(months) {
      var date_obj = {};
      months.forEach((month) => {
        date_obj[`${month}`] = {
          start_j: `${
            new Date()
              .toLocaleDateString("fa-IR")
              .replace(/([۰-۹])/g, (token) =>
                String.fromCharCode(token.charCodeAt(0) - 1728)
              )
              .split("/")[0]
          }-${month.toString().padStart(2, "0")}-01 00:00:00`,
          end_j: `${
            new Date()
              .toLocaleDateString("fa-IR")
              .replace(/([۰-۹])/g, (token) =>
                String.fromCharCode(token.charCodeAt(0) - 1728)
              )
              .split("/")[0]
          }-${month.toString().padStart(2, "0")}-30 23:59:59`,
          start_g: getGregorianDate(
            `${
              new Date()
                .toLocaleDateString("fa-IR")
                .replace(/([۰-۹])/g, (token) =>
                  String.fromCharCode(token.charCodeAt(0) - 1728)
                )
                .split("/")[0]
            }-${month.toString().padStart(2, "0")}-01 00:00:00`,
            "YYYY-MM-DD HH:mm:ss"
          ),
          end_g: getGregorianDate(
            `${
              new Date()
                .toLocaleDateString("fa-IR")
                .replace(/([۰-۹])/g, (token) =>
                  String.fromCharCode(token.charCodeAt(0) - 1728)
                )
                .split("/")[0]
            }-${month.toString().padStart(2, "0")}-30 23:59:59`,
            "YYYY-MM-DD HH:mm:ss"
          )
        };
      });
      this.$emit("update:modelValue", date_obj);
    }
  }
};
</script>

<style scoped>
.my-custom-card {
  height: 200px !important;
  width: 370px !important;
}
</style>
