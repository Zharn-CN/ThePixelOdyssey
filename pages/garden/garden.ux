<template>
  <div class="bg" @swipe="">
    <scroll id="scroll" scroll-x="true">
      <div class="fields">
        <div for="{{ dFields }}" class="field {{ $item.field }}" @click="onFieldPressed($item)">
          <image src="/common/garden/fence.png" class="fence" />
          <div class="state state-buy" if="{{ !$item.buy }}"></div>
          <div class="state state-arable" if="{{ $item.buy&&!$item.arable }}"></div>
          <div class="state state-water" if="{{  $item.buy&&!$item.water }}"></div>
          <image class="plant" if="{{ $item.plant!=null }}"
            src="/common/plants/{{$item.plant.key}}/{{$item.plant.state}}.png" />
          <div class="plant-ssz" if="{{ $item.plant != null && $item.plant.maxState > $item.plant.state }}"></div>
          <div class="plant-ok" if="{{ $item.plant != null && $item.plant.maxState == $item.plant.state }}"></div>
        </div>
        <div class="exit-btn" @click="exit()"></div>
      </div>
    </scroll>

    <div class="menu" if="{{ menu.open }}">
      <div class="menu-field {{ menu.item.field }}">
        <image class="menu-plant" src="/common/plants/{{menu.item.plant.key}}/{{menu.item.plant.state}}.png"
          if="{{menu.item.plant != null}}" />
      </div>
      <image src="/common/garden/fence.png" class="menu-fence" />
      <text class="menu-state-text">{{ menu.stateText }}</text>

      <div class="menu-gui" if="{{ !menu.item.buy }}">
        <text class="menu-buy-text">解锁所需</text>
        <text class="menu-buy-text2">德赛币x100</text>
      </div>

      <list class="menu-seed" if="{{ menu.item.buy && menu.item.water && menu.item.arable && menu.item.plant == null }}">
        <list-item for="{{ dPLANTS_DATA }}" class="menu-seed-item" @click="onMenuSeedItemPressed($item)" type="item">
          <div show="{{$item.name == menu.seed.name}}" class="menu-seed-item-sel"></div>
          <image class="menu-seed-item-img" src="/common/作物/{{$item.name}}/{{$item.maxState}}.png" />
          <text class="menu-seed-item-text">{{ $item.name }}</text>
          <text class="menu-seed-item-text2">{{ $item.buy }}</text>
        </list-item>
      </list>

      <div class="menu-rice menu-gui" if="{{ menu.item.plant.maxState == menu.item.plant.state }}">
        <text class="menu-rice-text">收获可得</text>
        <div class="menu-rice-things">
          <image class="menu-rice-img" src="/common/plants/{{menu.item.plant.key}}/{{menu.item.plant.maxState}}.png" />
          <text class="menu-rice-text1">{{ menu.item.plant.name }}</text>
          <text class="menu-rice-text2">x1</text>
        </div>
      </div>

      <div class="menu-gui" if="{{ menu.item.plant != null && menu.item.plant.maxState != menu.item.plant.state }}">
        <text class="menu-szz-text">下一阶段</text>
        <text class="menu-szz-text2">{{ menu.nextStateText }}</text>
      </div>

      <div class="menu-btns">
        <image class="menu-state-btn" src="/common/menu/bz.png"
          if="{{ menu.item.buy && menu.item.water && menu.item.arable && menu.item.plant == null }}"
          @click="onStatePressed(3)" />
        <image class="menu-state-btn" src="/common/menu/cc.png" if="{{ menu.item.plant != null }}"
          @click="onStatePressed(5)" />
        <image class="menu-state-btn" src="/common/menu/gd.png" if="{{ menu.item.buy&&!menu.item.arable }}"
          @click="onStatePressed(2)" />
        <image class="menu-state-btn" src="/common/menu/jiaoshui.png" if="{{ menu.item.buy&&!menu.item.water }}"
          @click="onStatePressed(1)" />
        <image class="menu-state-btn" src="/common/menu/js.png" if="{{ !menu.item.buy }}" @click="onStatePressed(0)" />
        <image class="menu-state-btn" src="/common/menu/sj.png"
          if="{{ menu.item.plant.maxState == menu.item.plant.state }}" @click="onStatePressed(4)" />
      </div>
      <image class="menu-close-btn" src="/common/menu/exit.png" @click="onCloseButtonPressed()" />
    </div>

    <div class="loading" if="{{ loading }}">
      <text class="loading-text">加载中</text>
      <text class="loading-text2">正在读取数据</text>
    </div>
  </div>
</template>
<script>
const STATE_BUY = 0
const STATE_WATER = 1
const STATE_ARABLE = 2
const STATE_SEED = 3
const STATE_COLLECT = 4
const STATE_KILL = 5

let intervalId = null
export default {
  private: {
    loading: true,
    menu: {
      open: false,
      item: null,
      stateText: "null",

      nextStateText: "反正没好",
      seed: null
    },

    dFields: null,
    dPLANTS_DATA: null
  },

  onMenuSeedItemPressed(item) {
    this.menu.seed = item
  },

  onCloseButtonPressed() {
    this.menu.open = false
  },

  exit() {
    this.$app.$def.jumpPage("main")
  },

  onInit() {
    let time = new Date().getTime()
    for (let i = 0; i < 8; i++) {
      let item = this.$app.$def.fields[i]
      if (
        item.plant != null &&
        item.plant.state < item.plant.maxState &&
        time - item.plant.lastTime >= item.plant.period
      ) {
        item.plant.state += parseInt((time - item.plant.lastTime) / item.plant.period)
        if (item.plant.state > item.plant.maxState) {
          item.plant.state = item.plant.maxState
          item.plant.lastTime = time
        }
      }
    }
    console.log("处理Fields成功")
    this.loading = false
    this.dFields = this.$app.$def.fields
    this.dPLANTS_DATA = this.$app.$def.data.PLANTS_DATA
    console.log("PLANTS_DATA get ")
  },

  onShow() {
    intervalId = setInterval(() => {
      let time = new Date().getTime()
      for (let i = 0; i < 8; i++) {
        let item = this.$app.$def.fields[i]
        if (
          item.plant != null &&
          item.plant.state < item.plant.maxState &&
          time - item.plant.lastTime >= item.plant.period
        ) {
          item.plant.state++
          item.plant.lastTime += item.plant.period
        }
      }
    }, 200)
  },

  onHide() {
    clearInterval(intervalId)
  },

  onFieldPressed(item) {
    this.menu.item = item
    if (!item.buy) {
      this.menu.stateText = "待解锁"
    } else if (!item.water) {
      if (!item.arable) {
        this.menu.stateText = "待耕地"
      } else {
        this.menu.stateText = "待浇水"
      }
    } else if (!item.arable) {
      this.menu.stateText = "待耕地"
    } else if (item.plant == null) {
      this.menu.stateText = "待播种"
      this.menu.seed = this.$app.$def.data.PLANTS_DATA[0]
    } else if (item.plant.maxState == item.plant.state) {
      this.menu.stateText = "待收集"
    } else {
      this.menu.stateText = "生长中"
      if (item.plant.maxState - item.plant.state == 1) {
        this.menu.nextStateText = "成熟"
      } else {
        this.menu.nextStateText = "没好反正"
      }
    }
    this.menu.open = true
  },

  onStatePressed(index) {
    if (index === STATE_BUY) {
      if (this.$app.$def.money >= 100) {
        this.$app.$def.money -= 100
        this.menu.item.buy = true
      }
    } else if (index === STATE_WATER) {
      this.menu.item.water = true
      if (this.menu.item.arable) {
        this.menu.item.field = "field-water-arable"
      } else {
        this.menu.item.field = "field-water"
      }
    } else if (index === STATE_ARABLE) {
      this.menu.item.arable = true
      if (this.menu.item.water) {
        this.menu.item.field = "field-water-arable"
      } else {
        this.menu.item.field = "field-arable"
      }
    } else if (index == STATE_SEED) {
      if (this.$app.$def.money >= this.menu.seed.buy) {
        this.$app.$def.money -= this.menu.seed.buy

        this.menu.stateText = "生长中"
        this.menu.item.plant = {
          name: this.menu.seed.name,
          lastTime: new Date().getTime(),
          state: 1,
          maxState: this.menu.seed.maxState,
          period: this.menu.seed.period,
          key: this.menu.seed.key,
          sell: this.menu.seed.sell
        }
        this.menu.seed.count--
        this.menu.open = false
      }
    } else if (index == STATE_COLLECT) {
      let tempBool = false
      for (let i = 0; i < this.$app.$def.plants.length; i++) {
        if (this.$app.$def.plants[i].name == this.menu.item.plant.name) {
          this.$app.$def.data.plants[i].count += 1
          tempBool = true
          break
        }
      }
      if (tempBool == false) {
        this.$app.$def.plants.push({
          name: this.menu.item.plant.name,
          key: this.menu.item.plant.key,
          maxState: this.menu.item.plant.maxState,
          count: 1,
          sell: this.menu.item.plant.sell
        })
      }
      this.menu.item.plant = null
    } else if (index == STATE_KILL) {
      this.menu.item.plant = null
    }

    let item = this.menu.item

    if (!item.buy) {
      this.menu.stateText = "待解锁"
    } else if (!item.water) {
      if (!item.arable) {
        this.menu.stateText = "待耕地"
      } else {
        this.menu.stateText = "待浇水"
      }
    } else if (!item.arable) {
      this.menu.stateText = "待耕地"
    } else if (item.plant == null) {
      this.menu.stateText = "待播种"
      this.menu.seed = this.$app.$def.plants[0]
    } else if (item.plant.maxState == item.plant.state) {
      this.menu.stateText = "待收集"
    } else {
      this.menu.stateText = "生长中"
      if (item.plant.maxState - item.plant.state == 1) {
        this.menu.nextStateText = "成熟"
      } else {
        this.menu.nextStateText = "没好反正"
      }
    }
  }
}
</script>

<style>
@import "../base.css";
@import "./garden.css";
</style>