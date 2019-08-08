<template>
  <el-container class="content">
    <el-aside width="346px">
      <el-container>
        <el-aside width="58px" class="outer-list">
          <div
            v-for="(item, index) in outerList"
            :key="item.id"
            class="outer-list-item"
            @click="changeOuterIndex(index)"
          >
            <img :src="outerSelectedIndex == index ? item.selectedSrc : item.src" alt />
            <p :class="outerSelectedIndex == index ? 'outerIsSeleted' : ''">{{item.name}}</p>
          </div>
        </el-aside>
        <el-main>
          <draggable
            v-if="outerSelectedIndex == 0"
            class="inner-list"
            v-model="chuntuList"
            v-bind="{ group: { name: 'people', pull: 'clone', put: false }, sort: false, ghostClass: 'ghost'}"
            @start="handleMoveStart"
            @end="handleMoveEnd"
            :move="handleMove"
          >
            <div v-for="(element, index) in chuntuList" :key="index" class="inner-list-item" :class="innerSelectedIndex == index ? 'innerIsSelected' : ''" @click="changeInnerIndex(index)">
              <p>{{element.name}}</p>
              <img :src="element.src" alt />
            </div>
          </draggable>
          <!-- <draggable
            v-if="outerSelectedIndex == 1"
            class="inner-list"
            v-model="tuwenList"
            v-bind="{ group: { name: 'people', pull: 'clone', put: false }, sort: false, ghostClass: 'ghost'}"
            @start="handleMoveStart"
            @end="handleMoveEnd"
            :move="handleMove"
          >
            <div v-for="(element, index) in tuwenList" :key="index" class="inner-list-item">
              <p>{{element.name}}</p>
              <img :src="element.src" alt />
            </div>
          </draggable> -->
          <draggable
            v-if="outerSelectedIndex == 1"
            class="inner-list"
            v-model="huadongList"
            v-bind="{ group: { name: 'people', pull: 'clone', put: false }, sort: false, ghostClass: 'ghost'}"
            @start="handleMoveStart"
            @end="handleMoveEnd"
            :move="handleMove"
          >
            <div
              v-for="(element, index) in huadongList"
              :key="index"
              class="inner-list-item"
              :class="innerSelectedIndex == index ? 'innerIsSelected' : ''"
              @click="changeInnerIndex(index)"
            >
              <p>{{element.name}}</p>
              <img :src="element.src" alt />
            </div>
          </draggable>
        </el-main>
      </el-container>
    </el-aside>
    <el-container class="visual-block">
      <el-header height="64px">
        <img src="data:image/png;base64,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" alt />
        <p class="title">{{pageTitle}}</p>
      </el-header>
      <el-main>
        <draggable
          class="visual-list"
          v-model="visualList"
          v-bind="{ group: { name: 'people' }, ghostClass: 'ghost', handle: '.handle'}"
          @end="handleMoveEnd"
          @add="addModule"
        >
          <div
            v-for="(element, index) in visualList"
            :key="index"
            class="visual-list-item"
            :class="moduleSelectedIndex == index ? 'moduleIsSelected' : ''"
            @click="choiceModule(element, index)"
            :style="{marginTop: element.block_style.ios.margin.top + 'px', marginLeft: element.block_style.ios.margin.left + 'px', marginBottom: element.block_style.ios.margin.bottom + 'px', marginRight: element.block_style.ios.margin.right + 'px', paddingTop: element.block_style.ios.padding.top + 'px', paddingLeft: element.block_style.ios.padding.left + 'px', paddingBottom: element.block_style.ios.padding.bottom + 'px', paddingRight: element.block_style.ios.padding.right + 'px', width: element.block_style.ios.size.width / 2 + 'px'}"
          >
            <template v-if="element">
              <template v-if="element.block_type == '17' || element.block_type == '8'">
                <div class="visual-list-item-chun" :style="{height: `${element.block_style.ios.size.height / 2}px`}">
                  <template v-for="(item, i) in element.block_items">
                    <el-image v-if="item.item_image != ''" :src="item.item_image" fit="contain" :key="i" alt="" :style="{width: `${(element.block_style.ios.size.width - 100) / 2 / 4}px`}"></el-image>
                    <div v-else :key="i" style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 100) / 2 / 4}px`, borderRadius: element.block_type == '17' ? '50%' : 0}"></div>
                  </template>
                </div>
              </template>
              <template v-if="element.block_type == '2'">
                <div class="visual-list-item-chun" :style="{height: `${element.block_style.ios.size.height / 2}px`}">
                  <template v-for="(item, i) in element.block_items">
                    <el-image v-if="item.item_image != ''" :src="item.item_image" fit="contain" :key="i" alt="" :style="{width: `${(element.block_style.ios.size.width - 80) / 2 / 3}px`}"></el-image>
                    <div v-else :key="i" style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 80) / 2 / 3}px`}"></div>
                  </template>
                </div>
              </template>
              <template v-if="element.block_type == '20'">
                <div class="visual-list-item-chun" :style="{height: `${element.block_style.ios.size.height / 2}px`}">
                  <template v-for="(item, i) in element.block_items">
                    <el-image v-if="item.item_image != ''" :src="item.item_image" fit="contain" :key="i" alt="" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}"></el-image>
                    <div v-else :key="i" style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}"></div>
                  </template>
                </div>
              </template>
              <template v-if="element.block_type == '14'">
                <div class="visual-list-item-chun" :style="{height: `${element.block_style.ios.size.height / 2}px`}">
                  <template>
                    <img v-if="element.block_items[0].item_image != ''" :src="element.block_items[0].item_image" style="object-fit: contain;" alt="" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}">
                    <div v-else style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}"></div>
                  </template>
                  <div :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}">
                    <img v-if="element.block_items[1].item_image != ''" :src="element.block_items[1].item_image" alt="" style="margin-bottom: 10px; object-fit: contain;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}">
                    <div v-else style="background-color: #d3dce6; margin-bottom: 10px;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}"></div>
                    <img v-if="element.block_items[2].item_image != ''" :src="element.block_items[2].item_image" style="object-fit: contain;" alt="" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}">
                    <div v-else style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}"></div>
                  </div>
                </div>
              </template>
              <template v-if="element.block_type == '29'">
                <div class="visual-list-item-chun" :style="{height: `${element.block_style.ios.size.height / 2}px`}">
                  <div :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}">
                    <img v-if="element.block_items[0].item_image != ''" :src="element.block_items[0].item_image" alt="" style="margin-bottom: 10px; object-fit: contain;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}">
                    <div v-else style="background-color: #d3dce6; margin-bottom: 10px;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}"></div>
                    <img v-if="element.block_items[1].item_image != ''" :src="element.block_items[1].item_image" style="object-fit: contain;" alt="" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}">
                    <div v-else style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`, height: `${(element.block_style.ios.size.height - 40) / 4 - 5}px`}"></div>
                  </div>
                  <template>
                    <img v-if="element.block_items[2].item_image != ''" :src="element.block_items[2].item_image" style="object-fit: contain;" alt="" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}">
                    <div v-else style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 60) / 2 / 2}px`}"></div>
                  </template>
                </div>
              </template>
              <template v-if="element.block_type == '24'">
                <div class="visual-list-item-chun" :style="{height: `${element.block_style.ios.size.height / 2}px`}">
                  <template v-for="(item, i) in element.block_items">
                    <el-image v-if="item.item_image != ''" :src="item.item_image" fit="contain" :key="i" alt="" :style="{width: `${(element.block_style.ios.size.width - 40) / 2}px`}"></el-image>
                    <div v-else :key="i" style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 40) / 2}px`}"></div>
                  </template>
                </div>
              </template>
              <template v-if="element.block_type == '1'">
                <el-carousel :height="`${element.block_style.ios.size.height / 2}px`">
                  <el-carousel-item v-for="(item, i) in element.block_items" :key="i">
                    <el-image v-if="item.item_image != ''" :src="item.item_image" fit="fill" alt style="width: 100%; height: 100%;"></el-image>
                    <div v-else style="width: 100%; height: 100%;" :style="{backgroundColor: i % 2 == 0 ? '#99a9bf' : '#d3dce6'}"></div>
                  </el-carousel-item>
                </el-carousel>
              </template>
              <template v-if="element.block_type == '16'">
                <div class="visual-list-item-hua" :style="{height: `${element.block_style.ios.size.height / 2 + 17}px`}">
                  <template v-for="(item, i) in element.block_items">
                    <el-image v-if="item.item_image != ''" :src="item.item_image" :key="i" fit="contain" alt="" :style="{width: `${(element.block_style.ios.size.width - 80) / 2 / 3}px`}"></el-image>
                    <div v-else :key="i" style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 80) / 2 / 3}px`}"></div>
                  </template>
                </div>
              </template>
              <template v-if="element.block_type == '19'">
                <div class="visual-list-item-hua" :style="{height: `${element.block_style.ios.size.height / 2 + 17}px`}">
                  <template v-for="(item, i) in element.block_items">
                    <el-image v-if="item.item_image != ''" :src="item.item_image" :key="i" fit="contain" alt="" :style="{width: `${(element.block_style.ios.size.width - 80) / 2 / 3.5}px`}"></el-image>
                    <div v-else :key="i" style="background-color: #d3dce6;" :style="{width: `${(element.block_style.ios.size.width - 80) / 2 / 3.5}px`}"></div>
                  </template>
                </div>
              </template>
              <i v-if="moduleSelectedIndex == index" class="handle"><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAATCAYAAACKsM07AAAAAXNSR0IArs4c6QAAAC9JREFUOBFjYGBg2ArE/2mEtzIBDaYlADl8FIyGwFAIgdGMNhRiadSNtA6BoZ0PAPcmSQD3jZPlAAAAAElFTkSuQmCC" alt=""></i>
              <div v-if="moduleSelectedIndex == index" class="operate">
                <img v-if="index != 0" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAQCAYAAAAmlE46AAAAAXNSR0IArs4c6QAAAQlJREFUKBXtki9vwlAUxc9pCAkjYWYTUxPzhI8w6mY2tQkyAWgcFo1DbckMIRWQ7I+pJDPIzUzuM2BALpCG9O4WKCm39BvwzHv3vN9p3rm9RMZyB+IK8AgHvUmdvxZzrBDV1560hfhUY0NCfFU9ubUck8L9uxRmf+iLoJbUQeh30Jk02I31ndEdyWUYwIegEl+mduL1vIjmxwMXa2OUJwTeFDxLwUYg8UMHd7wZSmkZYKp5TgyTXRJjighdD8+aq7wmiSt97oVxrbT+3t5Heb1dxhjUjj4hRCuuo12fN9fG7MU4+DuSpqzz0ZjVGdVTzaEgsLwOR0rLWSifw0uwwqkOwWaSNgPuW+4fwtZJoG9DMccAAAAASUVORK5CYII=" alt="" @click.stop="upModule(index, element)">
                <img v-if="index != visualList.length - 1" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAQCAYAAAAmlE46AAAAAXNSR0IArs4c6QAAAQdJREFUKBVjZEADs2fPDvr3798SoDAnTIqRkXFxWlpaHIwPopmQOSD2////HYEUXBNUzAtEIwMMjciS+NijGvGEDiNIbv78+RzAaBAFsf/8+dMEZCeA2DAAjMcPLCwseiD+379//6ekpDxlnDlzJheQfxeIJUASxACgQWtA0fEbiF8SowFJzV2m9PT030Bn2AJN2YgkgZUJVPMfKFEDTH4VYD+CVAH9xThnzpw6IF0PYmPR+YWJiSkmNTUVbAGGglmzZvkBNS4GauSDaQbadB+oyQ8YKFdgYhhJDuiMTUBJcyC+CVIE1HQAaJAZsiawOIjABlatWsXz8eNHSxUVlf2Ojo5/0NUAAF0NVyCVN/bdAAAAAElFTkSuQmCC" alt="" @click.stop="downModule(index, element)">
                <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAASCAYAAACEnoQPAAAAAXNSR0IArs4c6QAAAJFJREFUOBHlU9sNgCAMtMYJdCvdg51gD91KV0Du40iDjRD5tAlcX9fSRmUwJISwxxhXhkTkcM5ttIkjFY2aCH9pM9ckM1hDSU88U+W5lljG0yhXV+ey4Dcbz8cBu0VH3oQLoudu0cHpmvmP5LxtbI+Cr8fS6SOa5PQHLUzQOn3Erm0/OnvvIyvXMHfWc76RdN4NUmNAnA27Pf0AAAAASUVORK5CYII=" alt="" @click.stop="deleteModule(index)">
              </div>
            </template>
          </div>
        </draggable>
      </el-main>
      <el-footer height="100px">
        <el-button type="primary" @click="postModule">确定</el-button>
      </el-footer>
    </el-container>
    <el-aside width="375px">
      <el-container>
        <el-header height="70px">
          <div class="img-size-set">
            <div
              class="img-set"
              :class="imgSizeSetIndex == 0 ? 'setIsSelected' : ''"
              @click="imgSizeSetIndex = 0"
            >图片设置</div>
            <div
              class="size-set"
              :class="imgSizeSetIndex == 1 ? 'setIsSelected' : ''"
              @click="imgSizeSetIndex = 1"
            >尺寸设置</div>
          </div>
        </el-header>
        <el-main>
          <div v-if="moduleSelectedType != '' && moduleSelectedIndex != -1" class="set-scroll">
            <draggable
              v-model="visualList[moduleSelectedIndex].block_items"
              v-bind="{ group: { name: 'card' }, ghostClass: 'ghost', handle: '.handle'}"
              v-if="imgSizeSetIndex == 0"
              @end="handleCardMoveEnd"
            >
              <div v-for="(item, index) in visualList[moduleSelectedIndex].block_items" :key="index">
                <el-upload
                  class="avatar-uploader"
                  :action="uploadUrl"
                  :show-file-list="false"
                  :on-success="handleAvatarSuccess.bind(null, {'index': index, 'item': item})"
                >
                  <img v-if="item.item_image" :src="item.item_image" class="avatar" />
                  <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
                <el-form label-width="80px">
                  <el-form-item label="标题">
                    <el-input v-model="item.item_title"></el-input>
                  </el-form-item>
                  <el-form-item label="跳转地址">
                    <el-input v-model="item.item_location"></el-input>
                  </el-form-item>
                  <el-form-item label="小程序地址">
                    <el-input v-model="item.item_mini_location"></el-input>
                  </el-form-item>
                </el-form>
                <div class="add-remove" v-if="moduleSelectedType == '1' || moduleSelectedType == '16' || moduleSelectedType == '19'">
                  <img 
                    v-if="index == visualList[moduleSelectedIndex].block_items.length - 1"
                    src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAYAAAByDd+UAAAAAXNSR0IArs4c6QAAAM1JREFUSA1jdFr43/PfX4ZZDAwMMkBMS/CEiZkhjdFh3v/HQFtobRnMI0+Y6GgZyFIZkIV0BWRZ2O7CwADC5AAWcjRZyJKjC6KHLB+Sbx0Dw6iFlIQeVr2jQYo1WCgRxMiHoAxNbD7bn4jf6hPAUrpyD6oamsbhf1S7wDxQbYFNHItShBDMZ47zEWLEsmjqQ2yOGLUQW6hQJDYapBQFHzbNGCUNNkXoYqAShOTMCzWErIyP7gBS+AOSaJ6Q4kIK1T5hAjW/gYbQw1JwUx8AXakhMO+rVQsAAAAASUVORK5CYII=" 
                    alt 
                    @click="addCarousel(index)" />
                  <img
                    :style="{marginLeft: index != visualList[moduleSelectedIndex].block_items.length - 1 ? '0' : '34px'}"
                    v-if="moduleSelectedType == '16' ? index >= 2 : moduleSelectedType == '19' ? index >= 3 : visualList[moduleSelectedIndex].block_items.length > 1"
                    src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAYAAAByDd+UAAAAAXNSR0IArs4c6QAAALBJREFUSA1jdFr43/PfX4ZZDAwMMkBMS/CEiZkhjdFh3v/HQFtobRnMI09YYJYdSGJkhInSggZ67D/ILiZaGI7PzFEL8YUOWXKjQUpWsOHTNBqk+EKHLDlQ0YYCHOf/3woshLxQBMnkAAvLbfsTGb2RtdM6DkHlJwrA8CG6i1BUU4FDax9iOHHUQowgoVRgNEgpDUEM/aNBihEklArQvSHMBGp+A139hFKXE6Ef3NQHAAOpHihfpkQCAAAAAElFTkSuQmCC"
                    alt
                    @click="deleteCarousel(index)"
                  />
                </div>
                <i class="handle"><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAATCAYAAACKsM07AAAAAXNSR0IArs4c6QAAAC9JREFUOBFjYGBg2ArE/2mEtzIBDaYlADl8FIyGwFAIgdGMNhRiadSNtA6BoZ0PAPcmSQD3jZPlAAAAAElFTkSuQmCC" alt=""></i>
              </div>
            </draggable>
            <div v-else>
              <div class="size-item">
                <p>苹果手机</p>
                <el-row type="flex" justify="space-between">
                  <el-col :span="4">margin</el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">上</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.margin.top"
                          @input="changeStyle('marginTop', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">左</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.margin.left"
                          @input="changeStyle('marginLeft', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">下</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.margin.bottom"
                          @input="changeStyle('marginBottom', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">右</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.margin.right"
                          @input="changeStyle('marginRight', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                </el-row>
                <el-row type="flex" justify="space-between">
                  <el-col :span="4">padding</el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">上</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.padding.top"
                          @input="changeStyle('paddingTop', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">左</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.padding.left"
                          @input="changeStyle('paddingLeft', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">下</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.padding.bottom"
                          @input="changeStyle('paddingBottom', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">右</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.padding.right"
                          @input="changeStyle('paddingRight', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                </el-row>
                <el-row type="flex" justify="space-between">
                  <el-col :span="4">尺寸</el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between">
                      <el-col :span="3">宽</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.size.width"
                          @input="changeStyle('width', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between">
                      <el-col :span="3">高</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.ios.size.height"
                          @input="changeStyle('height', 'ios')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                </el-row>
              </div>
              <div class="size-item">
                <p>安卓手机</p>
                <el-row type="flex" justify="space-between">
                  <el-col :span="4">margin</el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">上</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.margin.top"
                          @input="changeStyle('marginTop', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">左</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.margin.left"
                          @input="changeStyle('marginLeft', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">下</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.margin.bottom"
                          @input="changeStyle('marginBottom', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">右</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.margin.right"
                          @input="changeStyle('marginRight', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                </el-row>
                <el-row type="flex" justify="space-between">
                  <el-col :span="4">padding</el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">上</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.padding.top"
                          @input="changeStyle('paddingTop', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">左</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.padding.left"
                          @input="changeStyle('paddingLeft', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">下</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.padding.bottom"
                          @input="changeStyle('paddingBottom', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                    <el-row type="flex" justify="space-between" style="margin-bottom: 10px;">
                      <el-col :span="3">右</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.padding.right"
                          @input="changeStyle('paddingRight', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                </el-row>
                <el-row type="flex" justify="space-between">
                  <el-col :span="4">尺寸</el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between">
                      <el-col :span="3">宽</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.size.width"
                          @input="changeStyle('width', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                  <el-col :span="9">
                    <el-row type="flex" justify="space-between">
                      <el-col :span="3">高</el-col>
                      <el-col :span="20">
                        <el-input
                          v-model="visualList[moduleSelectedIndex].block_style.android.size.height"
                          @input="changeStyle('height', 'android')"
                        >
                          <template slot="append">px</template>
                        </el-input>
                      </el-col>
                    </el-row>
                  </el-col>
                </el-row>
              </div>
            </div>
          </div>
        </el-main>
        <!-- <el-footer height="100px">
          <el-button>删除</el-button>
          <el-button type="primary">确定</el-button>
        </el-footer> -->
      </el-container>
    </el-aside>
  </el-container>
</template>

<script>
import draggable from "vuedraggable";
import {chuntuList, tuwenList, huadongList} from './componentsConfig.js'
export default {
  name: "adVisualEdit", // 广告可视化编辑
  components: {
    draggable
  },
  props: {
    list: Array,
    uploadUrl: String
  },
  data() {
    return {
      pageTitle: "标题",
      outerList: [
        {
          name: "N列纯图",
          src: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAAXNSR0IArs4c6QAAAKFJREFUSA3tlcENgCAMRcE4AEvoMLIBzAcb1GF0CTZA8AQVqTFeTOoJ+h8UfshXCP4IB2RL995DjHFpaVRNSrkaY3SXc87FLtAR8dqxZAFgCyFMuYbBkmuNlVK71nrGWtUgb56uh5lH82TreTAMD7jw9ZwbkI6yRWwR6QAJ/OsV5YBMcQ3ltaqwK4U3Y2vt5f9SNciRe5eKVMO0dqMY1psOHDX6J/I+G47yAAAAAElFTkSuQmCC',
          selectedSrc: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAAXNSR0IArs4c6QAAAKVJREFUSA1jYBgFBEKAEZu84/z/W///Z/DCJkdIjJGRYdv+REZvvOoc5gGNJxOg62VBNidx/f9b998zqILE0BUiq8PGVhRkuD0/kFENXQ7FApDh+xPRlRDHd5wPcRi6aiZ0AWrzRy0gGKKjQTQaRARDgKCCoZWKQAUksLjeiuwtlMIOWYIc9oEkoPFoAMUCUJGLq1RE04fBVRJkuHUAQ3RUgIgQAACtjiNssR/slQAAAABJRU5ErkJggg==',
          id: 1
        },
        // {
        //   name: "N列图文",
        //   src: require("./image/tuwen.png"),
        //   selectedSrc: require("./image/tuwen_selected.png"),
        //   id: 2
        // },
        {
          name: "滑动纯图",
          src: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAAXNSR0IArs4c6QAAAIFJREFUSA1jYBgFAx0CjMuWLdv6//9/LzwO+cHExJQAVBNHqjpGRsZtLCBN0dHRjLgsWLp0adq/f/+cgPIkqwPq/c+Ey2BqiY9aQDAkR4NoNIgIhgBBBaOpiGAQ0bq43krQBaMKGNetW/fz+/fvbMQEBScnJ4OJiQkxSsFqgFXmbQDHxkoT+B0PQQAAAABJRU5ErkJggg==',
          selectedSrc: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAAXNSR0IArs4c6QAAAJZJREFUSA1jYBgFAx0CjI7z/2/9/5/BC6dDGBl+MPxnSGBkZIgjVR1QzzYWkKYDSUAmDuC44H8a0AInctQ5zPv/nwmHuVQTHrWAYFCOBtFoEBEMAYIKRlMRwSBiARWpoFIPl0qgzA+gHKi4liFVHVDPVlzmjorDQ4DRd8m/n59/MbLBRfAw+Nj/MYQrf8ejAlUKWE3eBgDtKz0OblFdCQAAAABJRU5ErkJggg==',
          id: 3
        }
      ],
      outerSelectedIndex: 0,
      innerSelectedIndex: -1,
      chuntuList,
      tuwenList,
      huadongList,
      moduleSelectedIndex: -1,
      moduleSelectedType: "",
      visualList: this.list,
      imgSizeSetIndex: 0,
      imageUrl: ""
    };
  },
  computed: {
      
  },
  mounted() {
    
  },
  methods: {
    showList(data) {
      this.visualList = data;
    },
    // 选择N列纯图、N列图文、滑动纯图
    changeOuterIndex(index) {
      this.outerSelectedIndex = index;
      this.innerSelectedIndex = -1;
    },
    handleMoveEnd(evt) {
      // console.log("end", evt);
      this.moduleSelectedIndex = evt.newIndex;
      this.moduleSelectedType = this.visualList[evt.newIndex].block_type;
    },
    handleCardMoveEnd(evt) {
      // console.log("end", evt);
    },
    handleMoveStart({ oldIndex }) {
      // console.log("start", oldIndex, this.basicComponents);
    },
    handleMove() {
      return true;
    },
    addModule(evt) {
      let newIndex = evt.newIndex;
      this.$set(this.visualList, newIndex, {
        block_id: this.visualList[newIndex].block_id,
        block_type: this.visualList[newIndex].block_type,
        block_style: JSON.parse(JSON.stringify(this.visualList[newIndex].block_style)),
        block_enabled: this.visualList[newIndex].block_enabled,
        block_items: JSON.parse(JSON.stringify(this.visualList[newIndex].block_items))
      });
    },
    // 选择模块
    changeInnerIndex(index) {
      this.innerSelectedIndex = index;
    },
    // 选择对应的模块，对其进行操作
    choiceModule(element, index) {
      this.moduleSelectedType = element.block_type;
      this.moduleSelectedIndex = index;
    },
    upModule(index, element) {
      let item = this.visualList[index - 1];
      this.$set(this.visualList, index - 1, element);
      this.$set(this.visualList, index, item);
      this.moduleSelectedIndex--;
    },
    downModule(index, element) {
      let item = this.visualList[index + 1];
      this.$set(this.visualList, index + 1, element);
      this.$set(this.visualList, index, item);
      this.moduleSelectedIndex++;
    },
    deleteModule(index) {
      if (this.visualList.length == 1) {
        this.moduleSelectedIndex = -1;
        this.moduleSelectedType = '';
      } else if (this.visualList.length - 1 == index) {
        this.moduleSelectedIndex--;
        this.moduleSelectedType = this.visualList[this.moduleSelectedIndex].block_type;
      }

      this.visualList.splice(index, 1);
    },
    handleAvatarSuccess(obj, res, file) {
      console.log(obj, res, file)
      obj.item.item_image = res.d.file;
    },
    addCarousel(index) {
      this.visualList[this.moduleSelectedIndex].block_items.push({
        item_id: '0',
        item_order: `${index + 1}`,
        item_enabled: '1',
        item_title: `图${index + 2}`,
        item_image: '',
        item_location: '',
        item_mini_location: ''
      });
    },
    deleteCarousel(index) {
      this.visualList[this.moduleSelectedIndex].block_items.splice(index, 1);
    },
    changeStyle(str, system) {
      let android = this.visualList[this.moduleSelectedIndex].block_style.android,
        ios = this.visualList[this.moduleSelectedIndex].block_style.ios;
      if (system == "ios") {
        switch (str) {
          case "marginTop": {
            android.margin.top = ios.margin.top;
            break;
          }
          case "marginLeft": {
            android.margin.left = ios.margin.left;
            break;
          }
          case "marginBottom": {
            android.margin.bottom = ios.margin.bottom;
            break;
          }
          case "marginRight": {
            android.margin.right = ios.margin.right;
            break;
          }
          case "paddingTop": {
            android.padding.top = ios.padding.top;
            break;
          }
          case "paddingLeft": {
            android.padding.left = ios.padding.left;
            break;
          }
          case "paddingBottom": {
            android.padding.bottom = ios.padding.bottom;
            break;
          }
          case "paddingRight": {
            android.padding.right = ios.padding.right;
            break;
          }
          case "width": {
            android.size.width = ios.size.width;
            break;
          }
          case "height": {
            android.size.height = ios.size.height;
            break;
          }
        }
      } else {
        switch (str) {
          case "marginTop": {
            ios.margin.top = android.margin.top;
            break;
          }
          case "marginLeft": {
            ios.margin.left = android.margin.left;
            break;
          }
          case "marginBottom": {
            ios.margin.bottom = android.margin.bottom;
            break;
          }
          case "marginRight": {
            ios.margin.right = android.margin.right;
            break;
          }
          case "paddingTop": {
            ios.padding.top = android.padding.top;
            break;
          }
          case "paddingLeft": {
            ios.padding.left = android.padding.left;
            break;
          }
          case "paddingBottom": {
            ios.padding.bottom = android.padding.bottom;
            break;
          }
          case "paddingRight": {
            ios.padding.right = android.padding.right;
            break;
          }
          case "width": {
            ios.size.width = android.size.width;
            break;
          }
          case "height": {
            ios.size.height = android.size.height;
            break;
          }
        }
      }
    },
    postModule() {
      console.log(this.visualList);
      let visualData = [];
      this.visualList.map((item, index) => {
        let obj = {};
        obj.block_id = item.block_id;
        obj.block_type = item.block_type;
        obj.block_style = item.block_style;
        obj.block_enabled = item.block_enabled;
        obj.block_order = index;
        obj.block_items = [];
        item.block_items.map((items, i) => {
          let objs = {};
          objs.item_id = items.item_id;
          objs.item_order = i;
          objs.item_enabled = items.item_enabled;
          objs.item_title = items.item_title;
          objs.item_image = items.item_image;
          objs.item_location = items.item_location;
          objs.item_mini_location = items.item_mini_location;
          
          obj.block_items.push(objs);
        })

        visualData.push(obj);
      });

      console.log(visualData);
      let data = JSON.stringify(visualData);
      
      this.$emit('postModule', data);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
html,
body {
  margin: 0 auto;
  height: 100%;
}

img {
  display: block;
}

* {
  box-sizing: border-box;
}

p {
  margin-block-start: 0;
  margin-block-end: 0;
}

.content {
  width: 100%;
  height: 100%;
  background: #f4f4f4;
  .el-aside {
    background: #fff;

    .el-container {
      height: 100%;

      .el-header {
        padding: 20px 93.5px;
        .img-size-set {
          width: 188px;
          height: 30px;
          padding: 3px;
          border: 1px solid #ccc;

          .img-set {
            width: 90px;
            font-size: 12px;
            line-height: 22px;
            color: #666;
            float: left;
            cursor: pointer;
            text-align: center;
          }

          .size-set {
            width: 90px;
            font-size: 12px;
            line-height: 22px;
            color: #666;
            float: left;
            cursor: pointer;
            text-align: center;
          }

          .setIsSelected {
            background: #409eff;
            border-radius: 2px;
            color: #fff;
          }
        }
      }

      .el-main {
        padding: 0;
        overflow: hidden;
        position: relative;

        .set-scroll {
          position: absolute;
          left: 0;
          top: 0;
          right: -17px;
          bottom: 0;
          overflow-x: hidden;
          overflow-y: scroll;

          & > div {
            padding: 0 27.5px;

            & > div {
              position: relative;
            }
          }

          .handle {
            position: absolute;
            top: 5px;
            right: 5px;
            z-index: 2;
          }
        }

        .avatar-uploader .el-upload {
          border: 1px dashed #d9d9d9;
          border-radius: 6px;
          cursor: pointer;
          position: relative;
          overflow: hidden;
          margin-bottom: 12px;
        }
        .avatar-uploader .el-upload:hover {
          border-color: #409eff;
        }
        .avatar-uploader-icon {
          font-size: 28px;
          color: #8c939d;
          width: 320px;
          height: 120px;
          line-height: 120px;
          text-align: center;
        }
        .avatar {
          width: 320px;
          height: 120px;
          display: block;
        }
        .el-form-item {
          margin-bottom: 12px;
        }
        .el-form-item__label {
          font-size: 12px;
          color: #666;
        }

        .add-remove {
          display: flex;
          justify-content: center;
          margin-bottom: 20px;
        }

        .size-item {
          font-size: 12px;
          color: #6e6e6e;
          text-align: left;
          margin-bottom: 40px;

          p {
            margin-bottom: 18px;
          }

          .el-input-group__append {
            padding: 0 5px;
          }
        }
      }

      .el-footer {
        width: 375px;
        background: #fff;
        padding-top: 28px;
        text-align: center;

        .el-button {
          width: 140px;
          height: 32px;
          padding: 0;
        }
      }
    }
  }

  .outer-list {
    border-right: 1px solid #eee;
    height: 100%;

    .outer-list-item {
      height: 80px;
      width: 40px;
      margin: 0 9px 0;
      border-bottom: 1px solid #f0f0f0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;

      &:last-child {
        border-bottom: none;
      }

      p {
        width: 50px;
        font-size: 12px;
        line-height: 16px;
        color: #a5a5a5;
        margin-top: 5px;
        text-align: center;
      }

      .outerIsSeleted {
        color: #409eff;
      }
    }
  }

  .inner-list {
    position: absolute;
    left: 0;
    top: 0;
    right: -17px;
    bottom: 0;
    overflow-x: hidden;
    overflow-y: scroll;

    &::-webkit-scrollbar {
      display: none;
    }

    .inner-list-item {
      width: 288px;
      height: 130px;
      padding: 11px 30px 15px 18px;

      &:hover {
        background: #f6f6f6;
      }

      p {
        text-align: left;
        font-size: 12px;
        color: #666;
        line-height: 16px;
        margin-bottom: 7px;
      }

      div {
        width: 240px;
        height: 80px;
        border: 1px solid #dedede;
      }
    }

    .innerIsSelected {
      background: #f1f7fe;

      &:hover {
        background: #f1f7fe;
      }

      p {
        color: #409eff;
      }
    }
  }

  .visual-block {
    display: flex;
    align-items: center;

    .el-header {
      width: 375px;
      background: #fff;
      padding: 0;

      .title {
        font-size: 18px;
        color: #333;
        margin-top: 12px;
        font-weight: bold;
        text-align: center;
      }
    }

    .el-main {
      width: 375px;
      background: #fff;
      overflow: hidden;
      padding: 0;
      position: relative;

      .visual-list {
        position: absolute;
        left: 0;
        top: 0;
        right: -17px;
        bottom: 0;
        overflow-x: hidden;
        overflow-y: scroll;
        // height: 100%;

        .visual-list-item {
          position: relative;

          .el-carousel__item h3 {
            color: #475669;
            font-size: 14px;
            opacity: 0.75;
            line-height: 150px;
            margin: 0;
          }

          // .el-carousel__item:nth-child(2n) {
          //   background-color: #99a9bf;
          // }

          // .el-carousel__item:nth-child(2n + 1) {
          //   background-color: #d3dce6;
          // }

          .handle {
            position: absolute;
            top: 5px;
            right: 5px;
            z-index: 2;
          }

          .operate {
            z-index: 2;
            position: absolute;
            bottom: 5px;
            right: 5px;
            width: 107px;
            height: 30px;
            background: #FFFFFF;
            box-shadow: 0 0 2px 0 rgba(138,138,138,0.50);
            border-radius: 2px;
            display: flex;
            justify-content: space-around;
            align-items: center;

            & > img {
              width: 15px;
              height: 18px;
            }
          }
        }

        .moduleIsSelected {
          border: 2px solid #409eff;
        }

        .visual-list-item-chun {
          padding: 10px;
          white-space: nowrap;

          & > div {
            height: 100%;
            margin-right: 10px;
            display: inline-table;
          }

          & > div:last-child {
            margin-right: 0;
          }

          & > img {
            display: inline-table;
            margin-right: 10px;
            height: 100%;
          }

          & > img:last-child {
            margin-right: 0;
          }
        }

        .visual-list-item-hua {
          overflow-y: hidden;
          overflow-x: scroll;
          height: 100%;
          padding: 10px;
          white-space: nowrap;
            
          & > div {
            height: 100%;
            margin-right: 10px;
            display: inline-table;
          }
            
          & > div:last-child {
            margin-right: 0;
          }

          & > img {
            display: inline-table;
            margin-right: 10px;
            height: 100%;
          }

          & > img:last-child {
            margin-right: 0;
          }
        }
      }
    }

    .el-footer {
      width: 375px;
      background: #fff;
      padding-top: 28px;
      text-align: center;

      .el-button {
        width: 140px;
        height: 32px;
        padding: 0;
      }
    }
  }
}
</style>
