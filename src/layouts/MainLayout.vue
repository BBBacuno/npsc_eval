<template>
    <q-card
      class="evaluation"
      style="
        -webkit-border-radius: 50px;
        -moz-border-radius: 50px;
        border-radius: 50px;
      "
      id="topElement"
    >
    <div v-show="!verify">
      <div v-show="!verify" class="input-forms">
        <div class="button-container"><h4>Evaluation</h4></div>
        <br><br>
        <q-input
          outlined
          rounded
          v-model="formInput.OTPEmail"
          type="email"
          color="indigo-7"
          label-color="indigo-7"
          label="Registered Email for NPSC"
          :rules="[(val) => val.includes('@') || 'Only valid email']"
          lazy-rules
        >
          <template v-slot:before>
            <q-icon style="color: #3948ab85" name="mail" />
          </template>
        </q-input>
        <div class="button-container">
          <q-toggle
            v-model="accept"
            @click="accept ? (dataPolicyMenu = !dataPolicyMenu) : ''"
          >
            I accept the <b>Data Privacy Policy Terms</b>
          </q-toggle>
          <q-btn
            unelevated
            rounded
            size="lg"
            label="Start Evaluation"
            color="teal"
            :disabled="!accept"
            style="width: 50%"
            @click="sendOTP()"
          >
            <q-tooltip class="bg-white text-primary"
              >All Fields Required</q-tooltip
            >
          </q-btn>
        </div>
      </div>
    </div>
    <div v-show="verify" id="parentContainer">
        <q-form @submit="submitResponse()">
          <div v-show="pageNum == 1">
            <p style="justify-content: center;">
              <b>DIRECTIONS</b>: Using a 5-point Likert scale, please put a checkmark (✓) on the 
              appropriate column that represents the extent of your agreement to the following 
              statements about some quality dimensions of this activity. 
              <br>1 for agree, 5 for disagree
            </p>
            <q-separator />
            <br><br><b>{{ "Access and Facilities" }}</b>
            <div v-for="(item, index) in questions.af" :key="item.id">
              <br>{{ item }}
              <div class="radios">
                <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.af[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                  <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                        v-model="formInput.af[index]"
                        checked-icon="task_alt"
                        unchecked-icon="panorama_fish_eye"
                        :val="val"
                        :size="isScreenMD"
                        color="indigo-7"
                      >
                        <q-icon
                          style="color: rgba(57, 72, 171, 0.87)"
                          :name="smiley_icons[val-1]"
                          :size="isScreenMD"
                        />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 2">
            <br><b>{{ "Communication" }}</b>
              <div v-for="(item, index) in questions.c" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.c[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.c[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(57, 163, 171, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 3">
            <br><b>{{ "Quality" }}</b>
              <div v-for="(item, index) in questions.q" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.q[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.q[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(57, 171, 125, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 4">
            <br><b>{{ "Organization" }}</b>
              <div v-for="(item, index) in questions.org" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.org[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.org[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(165, 171, 57, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 5">
            <br><b>{{ "Outcome" }}</b>
              <div v-for="(item, index) in questions.out" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.out[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.out[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(120, 157, 10, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 6">
            <br><b>{{ "Aspect" }}</b>
              <div v-for="(item, index) in questions.aspect" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-select
                    color="indigo-7"
                    label-color="indigo-7"
                    v-model="formInput.aspectrate[index]"
                    class="text-fields-rate"
                    label="Rate"
                    :options="aspectrate_options"
                    emit-value
                    map-options
                    lazy-rules
                    :rules="[(val) => (val && val > 0) || 'Required Field']"
                  />
                  <q-input
                    rounded
                    outlined
                    color="indigo-7"
                    label-color="indigo-7"
                    class="text-fields-aspect"
                    v-model="formInput.aspectcomment[index]"
                    label="Comment"
                    lazy-rules
                    :rules="[
                      (val) => (val && val.length > 0) || 'Required Field',
                    ]"
                  />
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 7">
            <br><b>{{ "JAYEEL S. CORNELIO Ph. D." }}</b>
              <div v-for="(item, index) in questions.speakers" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.cornelio[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.cornelio[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(171, 112, 57, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 8">
            <br><b>{{ "ENGR. BERNIE C. CANGREJO" }}</b>
              <div v-for="(item, index) in questions.speakers" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.cangrejo[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.cangrejo[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(171, 57, 97, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 9">
            <br><b>{{ "PROF. MICHAEL CHARLESTON B. CHUA" }}</b>
              <div v-for="(item, index) in questions.speakers" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.chua[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.chua[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(169, 57, 171, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
            <q-separator />
          </div>
          <div v-show="pageNum == 10">
            <br><b>{{ "MS. ROSEJELYNN C. BULANTE " }}</b>
              <div v-for="(item, index) in questions.speakers" :key="item.id">
                <br>{{ item }}
                <div class="radios">
                  <q-field
                    class="answer-container"
                    borderless
                    ref="fieldRef"
                    v-model="formInput.bulante[index]"
                    stack-label
                    lazy-rules
                    :rules="[
                      (val) => (val > 0) || 'Required Field',
                    ]"
                  >
                    <div v-for="val in radio_val" :key="val.id">
                    <q-radio
                      v-model="formInput.bulante[index]"
                      checked-icon="task_alt"
                      unchecked-icon="panorama_fish_eye"
                      :val="val"
                      :size="isScreenMD"
                      color="indigo-7"
                    >
                      <q-icon
                        style="color: rgba(133, 57, 171, 0.87)"
                        :name="smiley_icons[val-1]"
                        :size="isScreenMD"
                      />
                    </q-radio>
                  </div>
                </q-field>
              </div>
            </div>
          </div>
          <div v-show="pageNum == 11">
            <br><b>{{ "OVERALL RATING" }}</b>
              <br>{{ questions.five1 }}
              <q-input
                rounded
                outlined
                color="indigo-7"
                label-color="indigo-7"
                class="text-fields-overall"
                v-model="formInput.five1"
                label="Comment"
                lazy-rules
                :rules="[
                  (val) => (val && val.length > 0) || 'Required Field',
                ]"
              />
              <br>{{ questions.five2 }}
              <q-input
                rounded
                outlined
                color="indigo-7"
                label-color="indigo-7"
                class="text-fields-overall"
                v-model="formInput.five2"
                label="Comment"
                lazy-rules
                :rules="[
                  (val) => (val && val.length > 0) || 'Required Field',
                ]"
              />
              <br>{{ questions.five3 }}
              <q-field
                class="answer-container"
                borderless
                ref="fieldRef"
                v-model="formInput.five3"
                stack-label
                lazy-rules
                :rules="[
                  (val) => (val > 0) || 'Required Field',
                ]"
              >
                <q-radio
                  v-model="formInput.five3"
                  checked-icon="task_alt"
                  unchecked-icon="panorama_fish_eye"
                  val="1"
                  :size="isScreenMD"
                  color="indigo-7"
                >
                  <q-icon
                    style="color: rgba(133, 57, 171, 0.87)"
                    name="check"
                    :size="isScreenMD"
                  />
                </q-radio>
                <q-radio
                  v-model="formInput.five3"
                  checked-icon="task_alt"
                  unchecked-icon="panorama_fish_eye"
                  val="2"
                  :size="isScreenMD"
                  color="indigo-7"
                >
                  <q-icon
                    style="color: rgba(133, 57, 171, 0.87)"
                    name="close"
                    :size="isScreenMD"
                  />
                </q-radio>
              </q-field>
              {{ formInput.five3 == 1 ? 'Why?' : 'Why not?'}}
              <div v-show="formInput.five3">
                <q-input
                  rounded
                  outlined
                  color="indigo-7"
                  label-color="indigo-7"
                  class="text-fields-overall"
                  v-model="formInput.five3comment"
                  label="Comment"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || 'Required Field',
                  ]"
                />
              </div>
              <br>{{ questions.five4 }}
              <q-input
                rounded
                outlined
                color="indigo-7"
                label-color="indigo-7"
                class="text-fields-overall"
                v-model="formInput.five4"
                label="Comment"
                lazy-rules
                :rules="[
                  (val) => (val && val.length > 0) || 'Required Field',
                ]"
              />
              <br>{{ questions.five5 }}
              <q-input
                rounded
                outlined
                color="indigo-7"
                label-color="indigo-7"
                class="text-fields-overall"
                v-model="formInput.five5"
                label="Comment"
                lazy-rules
                :rules="[
                  (val) => (val && val.length > 0) || 'Required Field',
                ]"
              />
          </div>
          <div class="button-container">
            <div class="radios" style="width: 100%;">
              <div v-show="pageNum > 1" style="width: 25%;">
                <q-btn label="Back" @click="pageNum--"  color="teal" class="button-submit" style="width: 100%;"></q-btn>
              </div>
              <div v-show="pageNum < 11" style="width: 25%;  ">
                <q-btn label="Next" @click="pageNum++, scrollToElement('#topElement')" color="teal" class="button-submit" style="width: 100%; height: 12%"></q-btn>
              </div>
            </div>
            <q-btn label="submit" type="submit" color="primary" class="button-submit"></q-btn>
          </div>
      </q-form>
    </div>
  </q-card>
  <q-dialog v-model="allRequired" persistent>
    <q-card style="width: 300px !important" class="prompt">
      <q-card-section class="q-pb-none text-center">
        <q-icon style="color: #3948ab85" name="error" size="70px" />
      </q-card-section>
      <q-card-section class="q-pa-md text-center">
        <div style="font-family: Montserrat; font-size: 25px">Ooops!</div>
      </q-card-section>

      <q-card-section
        class="q-pt-none text-center"
        style="font-family: Montserrat; font-size: 15px"
      >
        There are some unfulfilled details.
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="CLOSE" color="red-8" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
  <q-dialog v-model="pleaseWait" persistent>
    <q-card style="width: 300px" class="q-px-sm q-pb-md prompt">
      <q-card-section>
        <div style="font-family: Montserrat; font-size: 21px">
          Please wait . . .
        </div>
      </q-card-section>

      <q-card-section style="display: flex; justify-content: center">
        <q-spinner-dots color="grey-6" size="3em" />
      </q-card-section>
    </q-card>
  </q-dialog>
  <q-dialog v-model="congrats" persistent>
    <q-card class="prompt">
      <q-card-section class="q-pb-none text-center">
        <q-icon name="verified" color="green-5" size="70px" />
      </q-card-section>
      <q-card-section class="q-pa-md text-center">
        <div style="font-family: Montserrat; font-size: 25px">
          CONGRATULATIONS
        </div>
      </q-card-section>

      <q-card-section
        class="q-pt-none text-center"
        style="font-family: Montserrat; font-size: 15px"
      >
        Your
        <b>NPSC 2023</b> experience have been succesfully<br />concluded.
        an email will be sent to you containing<br />
        your certificate of participation. <br />

      </q-card-section>

      <q-card-actions align="right">
        <q-btn
          flat
          label="OK"
          color="green"
          @click="refreshPage()"
          v-close-popup
        />
      </q-card-actions>
    </q-card>
  </q-dialog>
  <q-dialog v-model="errorWarning" persistent>
    <q-card style="min-width: 350px" class="prompt">
      <q-card-section class="q-pb-none text-center">
        <q-icon style="color: #3948ab85" name="error" size="70px" />
      </q-card-section>
      <q-card-section class="q-pa-md text-center">
        <div style="font-family: Montserrat; font-size: 25px">Ooops!</div>
      </q-card-section>

      <q-card-section
        class="q-pt-none text-center"
        style="font-family: Montserrat; font-size: 15px"
      >
        Please try again
      </q-card-section>

      <q-card-actions align="right">
        <q-btn
          flat
          label="OK"
          color="grey"
          v-close-popup
        />
      </q-card-actions>
    </q-card>
  </q-dialog>
  <q-dialog v-model="notListed" persistent>
    <q-card style="min-width: 350px" class="prompt">
      <q-card-section class="q-pb-none text-center">
        <q-icon style="color: #3948ab85" name="error" size="70px" />
      </q-card-section>
      <q-card-section class="q-pa-md text-center">
        <div style="font-family: Montserrat; font-size: 25px">Ooops!</div>
      </q-card-section>

      <q-card-section
        class="q-pt-none text-center"
        style="font-family: Montserrat; font-size: 15px"
      >
        Kindly contact your respective scholarship coordinator to enlist your
        email.
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="OK" color="grey" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
  <q-dialog v-model="alreadySubmitted" persistent>
    <q-card style="min-width: 350px" class="prompt">
      <q-card-section class="q-pb-none text-center">
        <q-icon style="color: #3948ab85" name="error" size="70px" />
      </q-card-section>
      <q-card-section class="q-pa-md text-center">
        <div style="font-family: Montserrat; font-size: 25px">Ooops!</div>
      </q-card-section>

      <q-card-section
        class="q-pt-none text-center"
        style="font-family: Montserrat; font-size: 15px"
      >
        You have already submitted your evaluation forms.
        <br>Thank you.
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="OK" color="grey" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import { useQuasar } from "quasar";
import { ref, reactive } from 'vue';
import axios from 'axios';

const baseURL = import.meta.env.DEV ? "http://localhost/npsc_api" : "/npsc_api";
const axiosInit = axios.create({
  baseURL,
  withCredentials: true,
});

const formInput = reactive({
  af: [null],
  c: [null],
  q: [null],
  org: [null],
  out: [null],
  aspectrate: [null],
  aspectcomment: [null],
  cornelio: [null],
  cangrejo: [null],
  chua: [null],
  bulante: [null],
  five1: [null],
  five2: [null],
  five3: [null],
  five3comment: null,
  five4: [null],
  five5: [null],
  OTPEmail: null
});

const accept = ref(false)
// test verify
const verify = ref(false)
const otpSuccess = ref(null)
const otp = ref(null)
const isOTPSixDigit = ref(null)
const allRequired = ref(null)
const pleaseWait = ref(null)
const congrats = ref(null)
const errorWarning = ref(null)
const notListed = ref(null)
const isScreenMD = ref(null)
const pageNum = ref(1)
const alreadySubmitted = ref(null)

const refreshPage = () => {
  location.href = "/";
};

const scrollToElement = (el) => {
  const myDiv = document.querySelector(el);
  myDiv.scrollTo({top: 0, behavior: 'smooth'})
}


const sendOTP = () => {
  pleaseWait.value = true;
  axiosInit
    .get('/checkEmailforEval.php?email=' + formInput.OTPEmail)
    .then(function (response) {
      if (response.data.dupe === true){
        pleaseWait.value = false;
        alreadySubmitted.value = true
      } else if (response.data.success === true) {
        pleaseWait.value = false;
        verify.value = response.data.success;
      } else {
        pleaseWait.value = false;
        errorWarning.value = true;
      }
    });
};

export default {
  setup () {
    const $q = useQuasar();

    const toFormData = (obj) => {
      const formData = new FormData();
      for (const key in obj) {
        formData.append(key, obj[key]);
      }
      return formData;
    };
    
    const submitResponse = () => {
      pleaseWait.value = true
      const dlInsert = toFormData(formInput);
      axiosInit.post(
          "/submitEvaluation.php", dlInsert, {
            headers: {
              "Content-Type": "multipart/form-data",
            }
          }
        ).then(function (response) {
          if (response.data.success){
            pleaseWait.value = false
            congrats.value = true
          }
          else{
            pleaseWait.value = false
            errorWarning.value = true
          }
        })
    }

    return {
      scrollToElement,
      toFormData,
      submitResponse,
      sendOTP,
      refreshPage,
      formInput,
      accept,
      verify,
      otpSuccess,
      otp,
      isOTPSixDigit,
      allRequired,
      alreadySubmitted,
      pleaseWait,
      congrats,
      errorWarning,
      notListed,
      pageNum,
      isScreenMD: ref($q.screen.lt.md ? 'md' : 'xl'),
      radio_val: [1, 2, 3, 4, 5],
      aspectrate_options: [
        {
          label: '10 (Excellent/Highest)',
          value: 10
        },
        {
          label: '9',
          value: 9
        },
        {
          label: '8',
          value: 8
        },
        {
          label: '7',
          value: 7
        },
        {
          label: '6',
          value: 6
        },
        {
          label: '5 (Neutral)',
          value: 5
        },
        {
          label: '4',
          value: 4
        },
        {
          label: '3',
          value: 3
        },
        {
          label: '2',
          value: 2
        },
        {
          label: '1 (Poor/Lowest)',
          value: 1
        }
      ],
      smiley_icons: ['sentiment_very_satisfied', 'sentiment_satisfied_alt', 'sentiment_neutral', 'sentiment_dissatisfied', 'sentiment_very_dissatisfied', ],
      questions: {
        af: [
          "1. The venue was appropriate for the Congress.",
          "2. The sound system, audio-visual presentation, internet connection, and other technical tools used were working well.",
          "3. The online registration platform is effective and systematic.",
          "4. The activity kits and other materials needed for the event were all provided.",
          "5. Medical/hygiene kits were available and accessible throughout the conduct of the activity.",
          "6. Health protocol reminders were announced from time to time."
        ],
        c: [
          "Announcements and instructions were conveyed clearly.",
          "Queries about the activity were answered promptly.",
          "The organizers were knowledgeable in answering our queries.",
          "The theme and components of the activity were well-explained.",
          "We were encouraged to ask questions." 
        ],
        q: [
          "The 1st NPSC was systematic and well-organized.",
          "Every detail of the event was handled and managed well.",
          "The theme of the 1st NPSC is timely and relevant.",
          "There is enough time to finish the activities.",
          "The activities started on time.",
          "The media/visual aids and tools used in the activities helped me better understand the topics discussed."
        ],
        org: [
          "The organizers were well-prepared and knowledgeable in the conduct of the event.",
          "The organizers have precisely conceptualized and coordinated every detail of the event.",
          "The organizers were able to build rapport and a good working relationship with the participants.",
          "We felt safe and secure in providing information to the organizers.",
          "The organizers were accommodating and willing to help in any way they could.",
          "The organizers were polite and sincere in dealing with the participants' concerns.",
          "The security and welfare of the participants were considered in the conduct of the activities."
        ],
        out: [
          "The objectives of Congress were clearly defined.",
          "I gained new and important skills and knowledge from the activities.",
          "The topics were interesting, useful, and transformative.",
          "The Congress was a great avenue for fostering camaraderie and solidarity among science scholars.",
          "The activity taught me to fully understand and act on my responsibilities as a government scholar.",
          "The activity was effective at encouraging scholars to become engaged scientists and engineers for the betterment of the country."
        ],
        aspect: [
          "1. Plenary Talk: The Scientist as an Engaged Citizen",
          "2. The Patriot Scholar as an Engaged Citizen",
          "3. Viewing of Exhibits",
          "4. Panel Presentation: The Scientists as Servant Leaders",
          "5. Regional Workshop: Enabler - Obstruction ",
          "6. Meet Your Scientists",
          "7. Breakout Sessions: Oral Presentation of Volunteering Experiences",
          "8. Networking Workshop"
        ],
        speakers: [
          "Mastery of the subject matter",
          "Knowledgeable about the topic",
          "Presented topics clearly and audibly",
          "Responded effectively to questions",
          "Covered all necessary topics",
          "Time management"
        ],
        five1: "1. How would you rate the overall conduct of the Congress?",
        five2: "What important insight/s did you get from the Congress? ",
        five3: "Are your expectations in the Congress met?",
        five4: "Please indicate your comments/suggestions for improvement, if there are any.",
        five5: "Will you recommend this Congress in the future? Why?"
      }
    }
  }
}

</script>

<style>
.evaluation {
  padding-block: 55px;
  padding-inline: 15px;
  box-shadow: none;
  width: 60vw;
  margin-inline: auto;
  /* min-width: 40vw; */
  height: 100vh;
  -webkit-backdrop-filter: blur(8px); /* Safari 9+ */
  backdrop-filter: blur(8px); /* Chrome and Opera */
  box-shadow: 0px 10px 15px 10px rgb(0 0 0 / 15%);
  background-color: rgba(228, 228, 228, 0.627);
  -webkit-border-top-left-radius: 25px;
  -webkit-border-bottom-left-radius: 25px;
  -moz-border-radius-topleft: 25px;
  -moz-border-radius-bottomleft: 25px;
  border-top-left-radius: 25px;
  border-bottom-left-radius: 25px;
  /* border: 5px solid black; */
  font-family: montserrat;
  overflow: scroll;
  font-size: 1.3em;
}
@media screen and (max-width: 850px) {
  .evaluation {
    width: 100vw;
  }
}

body{
  background: radial-gradient(
    circle at 0% 49%,
    rgba(32, 25, 111, 1) 0%,
    rgba(32, 25, 111, 1) 25%,
    rgba(165, 170, 40, 1) 72%,
    rgba(247, 231, 52, 1) 100%
  );
}

.radios {
  /* border: 2px solid black; */
  min-width: 100%;
  padding-inline: 2%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

.button-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}

.button-container > * {
  margin: auto;
  margin-top: 2%;
  height: 100%;
}

::-webkit-scrollbar {
  width: 10px;
}

::-webkit-scrollbar-track {
  opacity: 0;
  margin-top: 40px;
  margin-bottom: 40px;
}

::-webkit-scrollbar-thumb {
  background-color: rgb(228, 228, 228);
  border-radius: 10px;
  width: 30px;
  background-clip: padding-box;
}
.answer-container{
  width: 80%;
}
.q-field__control-container{
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-evenly;
}
.button-submit{
  width: 50%;
}
.text-fields-rate{
  width: 25%;
}
.text-fields-aspect{
  width: 70%;
}
.text-fields-overall{
  width: 90%;
}
</style>
