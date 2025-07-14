<template>
  <div>
    <div style="margin-top: 20px">
      <div class="columns">
        <div class="column">
          <div
            class="field is-grouped is-grouped-multiline is-justify-content-flex-end"
          >
            <!-- <p class="control">
              <button @click="addHosokekhai" class="button is-small is-success">
                <span class="icon">
                  <i class="fas fa-pen-nib"></i>
                </span>
                <span>Nhập dữ liệu kê khai</span>
              </button>
            </p> -->
            <p class="control">
              <button @click="importKekhai" class="button is-small is-info">
                <span class="icon">
                  <i class="fas fa-file-import"></i>
                </span>
                <span>Import kê khai</span>
              </button>
            </p>
            <p class="control">
              <button class="button is-small is-warning">
                <span class="icon">
                  <i class="fas fa-file-download"></i>
                </span>
                <span>Tải về mẫu Import</span>
              </button>
            </p>
          </div>
        </div>
      </div>
      <div class="table_wrapper">
        <table
          class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth"
        >
          <thead style="font-weight: bold">
            <tr style="font-size: small; background-color: #fff8dc">
              <td style="text-align: center">Chức năng</td>
              <td style="text-align: center; width: 3%">STT</td>
              <td style="text-align: center">Mã số BHXH</td>
              <td style="text-align: center">Họ tên</td>
              <td style="text-align: center">Ngày sinh</td>
              <td style="text-align: center">Giới tính</td>
              <td style="text-align: center">CCCD</td>
              <td style="text-align: center">Điện thoại</td>
              <td style="text-align: center">Phương án</td>
              <td style="text-align: center">Lương cơ sở</td>
              <td style="text-align: center">Tỷ lệ NSTW %</td>
              <td style="text-align: center">Tỷ lệ NSĐP %</td>
              <td style="text-align: center">Tỷ lệ HT khác</td>
              <td style="text-align: center">Hạn thẻ cũ</td>
              <td style="text-align: center">Từ tháng</td>
              <td style="text-align: center">Số tháng</td>
              <td style="text-align: center">Số tiền phải đóng</td>
              <td style="text-align: center">Tỉnh / Thành phố</td>
              <!-- <td style="text-align: center">Quận / Huyện</td> -->
              <td style="text-align: center">Xã phường</td>
              <td style="text-align: center">Tổ thôn</td>
              <!--<td style="text-align: center">Bệnh viện tỉnh</td> -->
              <td style="text-align: center;">Bệnh viện</td>
              <td style="text-align: center">Hình thức nạp</td>
              <td style="text-align: center">Ghi chú</td>
              <!-- <td style="text-align: center">Số biên lai</td>
              <td style="text-align: center">Ngày biên lai</td> -->
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in items" :key="index">
              <td style="text-align: center; vertical-align: middle">
                <!-- <a @click="copyRow()">
                  <span class="icon is-small">
                    <i
                      style="color: hsl(153deg, 53%, 53%)"
                      class="fas fa-check-circle"
                    ></i>
                  </span>
                </a> -->
                &nbsp;
                <a @click="deleteRow(index)" title="Xóa bản ghi">
                  <span class="icon is-small">
                    <i style="color: red" class="far fa-trash-alt"></i>
                  </span>
                </a>
                &nbsp;
                <a @click="checkItem(item)" title="Check thông tin nhập">
                  <span class="icon is-small">
                    <i style="color: #198754" class="fab fa-angellist"></i>
                  </span>
                </a>
                &nbsp;
                <a @click="checkHgd(item)" title="Thông tin HGĐ">
                  <span class="icon is-small">
                    <i style="color: #0d6efd" class="fab fa-audible"></i>
                  </span>
                </a>
              </td>
              <td style="text-align: center; vertical-align: middle">
                {{ index + 1 }}
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.masobhxh"
                  class="input is-small"
                  type="number"
                  :ref="'masobhxhInput_' + item._id"
                  @blur="findNguoihuong(item.masobhxh, index)"
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.hoten"
                  class="input is-small"
                  type="text"
                  :ref="'nameInput' + item._id"
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.ngaysinh"
                  class="input is-small"
                  :ref="'ngaysinhInput' + item._id"
                />
              </td>
              <td style="text-align: center">
                <div class="select is-fullwidth is-small">
                  <select v-model="item.gioitinh">
                    <!-- Bind v-model để liên kết giá trị -->
                    <option value="" selected>- Chọn giới tính -</option>
                    <!-- Tùy chọn mặc định -->
                    <option value="Nam">Nam</option>
                    <option value="Nữ">Nữ</option>
                  </select>
                </div>
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.cccd"
                  class="input is-small"
                  type="number"
                  :ref="'cccdInput' + item._id"
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.dienthoai"
                  class="input is-small"
                  type="number"
                />
              </td>
              <td style="text-align: center">
                <div class="select is-fullwidth is-small">
                  <select
                    v-model="item.maphuongan"
                    @change="phuonganChange($event, index)"
                    :ref="'phuonganSelect' + item._id"
                  >
                    <option
                      v-for="(pa, index) in item.info_phuongan"
                      :key="index"
                      :value="pa.maphuongan"
                    >
                      {{ pa.tenphuongan }}
                    </option>
                  </select>
                </div>
              </td>

              <td style="text-align: center">
                <input
                  v-model="item.tienluongcs"
                  class="input is-small"
                  type="text"
                  v-mask="mask"
                  disabled
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.tylensnnht"
                  class="input is-small"
                  type="number"
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.tylensdp"
                  class="input is-small"
                  type="number"
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.hotrokhac"
                  class="input is-small"
                  type="number"
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.hanthecu"
                  type="text"
                  class="input is-small"
                />
              </td>
              <td style="text-align: center">
                <input
                  v-model="item.tungay"
                  type="text"
                  class="input is-small"
                />
              </td>
              <td style="text-align: center">
                <div class="select is-fullwidth is-small">
                  <select
                    v-model="item.maphuongthucdong"
                    @change="phuongthucdChange($event, index)"
                    :ref="'phuongthucdongSelect' + item._id"
                  >
                    <option selected disabled>- Chọn phương thức đóng -</option>
                    <option
                      v-for="(ptd, index) in item.phuongthucdong"
                      :key="index"
                      :value="ptd.maphuongthuc"
                    >
                      {{ ptd.tenphuongthuc }}
                    </option>
                  </select>
                </div>
              </td>
              <td style="text-align: center">
                <input
                  v-mask="mask"
                  v-model="item.sotien"
                  class="input is-small"
                  style="font-weight: 800; color: red"
                  disabled
                />
              </td>
              <!-- tỉnh-->
              <!-- <td style="text-align: center">
                <div class="select is-fullwidth is-small">
                  <select
                    v-model="item.matinh"
                    @change="provinceChange($event, index)"
                  >
                    <option
                      v-for="(dt, index) in dmtinhthanhpho"
                      :key="index"
                      :value="dt.matinh"
                    >
                      {{ dt.tentinh }}
                    </option>
                  </select>
                </div>
              </td> -->
              <td style="text-align: center">
                <div class="select is-fullwidth is-small">
                  <select
                    v-model="item.matinh"
                    @change="provinceChange($event, index)"
                  >
                    <option
                      v-for="(dt, index) in cq2cap_Tinh"
                      :key="index"
                      :value="dt.province_code"
                    >
                      {{ dt.name }}
                    </option>
                  </select>
                </div>
              </td>
              <!-- quận huyện -->
              <!-- <td style="text-align: center">
                <div class="select is-fullwidth is-small">
                  <select
                    @change="quanhuyenChange($event, index)"
                    :ref="'quanhuyenSelect' + item._id"
                  >
                    <option selected disabled>
                      {{ item.maquanhuyen }} -
                      {{ item.tenquanhuyen }}
                    </option>
                    <option
                      v-for="(dt, index) in item.info_huyen"
                      :key="index"
                      :value="dt.maquanhuyen"
                    >
                      {{ dt.tenquanhuyen }}
                    </option>
                  </select>
                </div>
              </td> -->
              <!-- xã phường -->
              <!-- <td style="text-align: center">
                <div class="select is-fullwidth is-small">
                  <select
                    @change="xaphuongChange($event, index)"
                    :disabled="isDisabled_Xaphuong"
                    :ref="'xaphuongSelect' + item._id"
                  >
                    <option selected disabled>
                      {{ item.maxaphuong }} -
                      {{ item.tenxaphuong }}
                    </option>
                    <option
                      v-for="(dt, index) in item.info_xaphuong"
                      :key="index"
                      :value="dt.maxaphuong"
                    >
                      {{ dt.tenxaphuong }}
                    </option>
                  </select>
                </div>
              </td> -->
              <td style="text-align: center">
                <!-- <div class="select is-fullwidth is-small">
                  <select
                    @change="xaphuongChange($event, index)"
                    :ref="'xaphuongSelect' + item._id"
                  >
                    <option selected disabled>
                      {{ item.maxaphuong }} -
                      {{ item.tenxaphuong }}
                    </option>
                    <option
                      v-for="(dt, index) in item.info_xaphuong"
                      :key="index"
                      :value="dt.ward_code"
                    >
                      {{ dt.ward_name }}
                    </option>
                  </select>
                </div> -->
                <!-- <v-select
                  v-model="item.ward_code"
                  :options="item.info_xaphuong"
                  label="ward_name"
                  :reduce="b => b.ward_code"
                  @input="xaphuongChange($event, index)"
                  :append-to-body="true"
                /> -->
                <v-select
                  v-model="item.maxaphuong"
                  :options="item.info_xaphuong"
                  label="ward_name"
                  :reduce="b => b.ward_code"
                  :get-option-label="val => {
                    if (typeof val === 'string') {
                      const found = item.info_xaphuong.find(x => x.ward_code === val);
                      return found ? found.ward_name : val;
                    }
                    return val.ward_name;
                  }"
                  @input="xaphuongChange($event, index)"
                  :append-to-body="true"
                />

              </td>
              <!-- tổ thôn -->
              <td style="text-align: center">
                <input
                  v-model="item.tothon"
                  class="input is-small"
                  type="text"
                  :ref="'tothonInpu' + item._id"
                />
              </td>

              <td style="text-align: center">
                <!-- <div class="select is-fullwidth is-small">
                  <select
                    v-model="item.mabenhvien"
                    @change="hopChangeReset($event, index)"
                  >
                    <option
                      v-for="(nt, idx) in item.info_benhvien"
                      :key="idx"
                      :value="nt.mabenhvien"
                    >
                      {{ nt.tenbenhvien }}
                    </option>
                  </select>
                </div> -->
                <v-select
                  v-model="item.mabenhvien"
                  :options="item.info_benhvien"
                  label="tenbenhvien"
                  :reduce="b => b.mabenhvien"
                  placeholder="Chọn trạm y tế"
                  @input="hopChangeReset($event, index)"
                  :append-to-body="true"
                />

              </td>

              <td>
                <div class="select is-fullwidth is-small">
                  <select
                    v-model="item.hinhthucnap"
                    @change="hinhthucNap($event, index)"
                    :ref="'hinhthucnapInput' + item._id"
                  >
                    <option value="0">Tiền mặt</option>
                    <option value="1">Chuyển khoản</option>
                  </select>
                </div>
              </td>

              <!-- ghi chú -->
              <td style="text-align: center">
                <input
                  v-model="item.ghichu"
                  class="input is-small"
                  type="text"
                />
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="button-container">
        <!-- Các nút thêm dòng và gửi kê khai -->
        <button @click="addRow" class="button is-info is-small">
          <span class="icon is-small">
            <i class="fas fa-plus"></i>
          </span>
          <span>Thêm dòng</span>
        </button>
        &nbsp;
        <button @click="guiKekhai" class="button is-danger is-small">
          <span class="icon is-small">
            <i class="fas fa-envelope-open-text"></i>
          </span>
          <span>Nạp kê khai</span>
        </button>

        <!-- Tổng số tiền, nằm bên phải -->
        <div class="total-sotien">
          Tổng số tiền:
          <span style="font-weight: 900; color: red">{{
            formatCurrency(totalSoTien)
          }}</span>
        </div>
      </div>
    </div>

    <!-- Biểu tượng loading -->
    <div v-if="isLoading" class="loading-overlay">
      <!-- Biểu tượng loading -->
      <div class="loading-spinner"></div>
      <span>waitting some minute ...</span>
    </div>

    <!-- modal nhập xem trước khi xác nhận gửi hồ sơ đi -->
    <div class="">
      <div :class="{ 'is-active': isActive_xacnhan }" class="modal">
        <div class="modal-background"></div>
          <div class="modal-content modal-card-predata">
            <section class="modal-card-body box">
            <div>
              <div>
                <span style="font-weight: 800; font-size: 15px; color: red"
                  >Xác nhận và xem biên lai (Bắt buộc bấm xác nhận để lưu trữ
                  biên lai điện tử vào dữ liệu)</span
                >
              </div>
              <div style="text-align: end"></div>
            </div>
            <div v-if="items.length > 0">
              <div class="titleKk">
                <hr class="line" />
                <div class="topleft">
                  <span style="color: red; font-weight: 700">1.</span> Thông tin
                  hồ sơ kê khai đã nhập
                </div>
              </div>

              <div class="table_wrapper">
                <table
                  class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth"
                >
                  <thead style="font-weight: bold">
                    <tr style="font-size: small; background-color: #fff8dc">
                      <td style="text-align: center; width: 3%">STT</td>
                      <td style="text-align: center">Biên lai</td>
                      <td style="text-align: center">Số biên lai</td>
                      <td style="text-align: center">Mã số BHXH</td>
                      <td style="text-align: center">Họ tên</td>
                      <td style="text-align: center">Ngày sinh</td>
                      <td style="text-align: center">Giới tính</td>
                      <td style="text-align: center">CCCD</td>
                      <td style="text-align: center">Điện thoại</td>
                      <td style="text-align: center">Loại hình</td>
                      <td style="text-align: center">Số tiền</td>
                      <td style="text-align: center">Số tháng đóng</td>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="(item, index) in dulieuTravedeinbienlai"
                      :key="index"
                      style="font-size: small"
                    >
                      <td style="text-align: center; vertical-align: middle">
                        {{ index + 1 }}
                      </td>
                      <td style="text-align: center">
                        <a @click="xemBienLai(item)">
                          <span
                            style="color: #ff69b4"
                            class="icon is-small is-left"
                          >
                            <i class="fas fa-print"></i>
                          </span>
                        </a>
                      </td>
                      <td style="text-align: center; font-weight: 500">
                        {{ item.sobienlai }}
                      </td>
                      <td style="text-align: center; font-weight: 500">
                        {{ item.masobhxh }}
                      </td>
                      <td style="text-align: left; font-weight: 500">
                        {{ item.hoten }}
                      </td>
                      <td style="text-align: center">
                        {{ item.ngaysinh }}
                      </td>
                      <td style="text-align: center">
                        {{ item.gioitinh }}
                      </td>
                      <td style="text-align: center; font-weight: 500">
                        {{ item.cccd }}
                      </td>
                      <td style="text-align: center; font-weight: 500">
                        {{ item.dienthoai }}
                      </td>
                      <td style="text-align: center">
                        {{ item.maloaihinh }}
                      </td>
                      <td style="text-align: center">
                        {{ item.sotien }}
                      </td>
                      <td style="text-align: center">
                        {{ item.tenphuongthucdong }}
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <div class="titleKk">
                <hr class="line" />
                <div class="topleft">
                  <span style="color: red; font-weight: 700">2.</span> Tổng số
                  tiền phải nạp:
                  <span style="color: red; font-weight: 700">{{
                    formatCurrency(totalSoTien)
                  }}</span>
                </div>
              </div>
              <hr class="navbar-divider" />
              <div class="columns">
                <div class="column" style="margin-top: 10px">
                  <div
                    class="field is-grouped is-flex is-justify-content-center"
                  >
                    <div class="control">
                      <button
                        @click="onSave"
                        class="button is-success is-small"
                        :disabled="lockButtonXacnhaninbldt"
                      >
                        Xác nhận biên lai
                      </button>
                    </div>
                    <div class="control">
                      <button
                        @click="hoanTatDongHs"
                        :disabled="!lockButtonXacnhaninbldt"
                        class="button is-warning is-light is-small"
                      >
                        Đóng
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </section>
        </div>
      </div>
    </div>

    <!-- modal import dữ liệu kê khai -->
    <div class="">
      <div :class="{ 'is-active': isActive_import }" class="modal">
        <div class="modal-background"></div>
        <div class="modal-content modal-card-kekhai box">
          <section class="modal-card-kekhai-body">
            <div>
              <div>
                <span style="font-weight: 800; font-size: 15px; color: #3cb371"
                  >Import dữ liệu kê khai</span
                >
              </div>
              <div style="text-align: end">
                <button
                  @click="isActive_import = false"
                  class="button is-small is-info"
                >
                  Thoát
                </button>
              </div>
            </div>
            <div>
              <div class="titleKk">
                <hr class="line" />
                <div class="topleft">
                  <span style="color: red; font-weight: 700">1.</span> Chọn file
                  dữ liệu
                </div>
              </div>
              <div class="columns">
                <div class="column">
                  <div>
                    <div class="file is-info has-name is-small">
                      <label class="file-label">
                        <input
                          @change="onFileChange"
                          class="file-input"
                          type="file"
                          name="resume"
                        />
                        <span class="file-cta">
                          <span class="file-icon">
                            <i class="fas fa-upload"></i>
                          </span>
                          <span class="file-label"> Chọn file dữ liệu </span>
                        </span>
                        <span class="file-name">
                          {{ fileName }}
                        </span>
                      </label>
                    </div>
                  </div>
                </div>
              </div>

              <div class="titleKk" style="margin-top: 10px">
                <hr class="line" />
                <div class="topleft">
                  <span style="color: red; font-weight: 700">2.</span> Danh sách
                  kê khai
                </div>
              </div>
              <div class="columns table_wrapper">
                <div class="column">
                  <div v-if="items.length > 0">
                    <table
                      class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth"
                    >
                      <thead>
                        <tr style="font-size: small; background-color: #fff8dc">
                          <td style="text-align: center; width: 3%">STT</td>
                          <td style="text-align: center">Mã số BHXH</td>
                          <td style="text-align: center">Họ tên</td>
                          <td style="text-align: center">Ngày sinh</td>
                          <td style="text-align: center">Giới tính</td>
                          <td style="text-align: center">CCCD</td>
                          <td style="text-align: center">Điện thoại</td>
                          <td style="text-align: center">Phương án</td>
                          <td style="text-align: center">Lương cơ sở</td>
                          <td style="text-align: center">Tỷ lệ NSTW %</td>
                          <td style="text-align: center">Tỷ lệ NSĐP %</td>
                          <td style="text-align: center">Tỷ lệ HT khác</td>
                          <td style="text-align: center">Từ ngày</td>
                          <td style="text-align: center">Số tháng</td>
                          <td style="text-align: center">Số tiền phải đóng</td>
                          <td style="text-align: center">Tỉnh / Thành phố</td>
                          <td style="text-align: center">Quận / Huyện</td>
                          <td style="text-align: center">Xã phường</td>
                          <td style="text-align: center">Tổ thôn</td>
                          <td style="text-align: center">Bệnh viện tỉnh</td>
                          <td style="text-align: center">Bệnh viện</td>
                          <td style="text-align: center">Ghi chú</td>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="(item, index) in items"
                          :key="index"
                          style="font-size: small"
                        >
                          <td
                            style="text-align: center; vertical-align: middle"
                          >
                            {{ index + 1 }}
                          </td>
                          <td style="text-align: center; font-weight: 500">
                            {{ item.masobhxh }}
                          </td>
                          <td style="text-align: center; font-weight: 500">
                            {{ item.hoten }}
                          </td>
                          <td style="text-align: center">
                            {{ formatISODateToDMY(item.ngaysinh) }}
                          </td>
                          <td style="text-align: center">
                            {{ item.gioitinh }}
                          </td>
                          <td style="text-align: center; font-weight: 500">
                            {{ item.cccd }}
                          </td>
                          <td style="text-align: center; font-weight: 500">
                            {{ item.dienthoai }}
                          </td>
                          <td style="text-align: center">
                            {{ item.tenphuongan }}
                          </td>
                          <td style="text-align: center">
                            {{ item.tienluongcs }}
                          </td>
                          <td style="text-align: center">
                            {{ item.tylensnnht }}
                          </td>
                          <td style="text-align: center">
                            {{ item.tylensdp }}
                          </td>
                          <td style="text-align: center">
                            {{ item.hotrokhac }}
                          </td>
                          <td style="text-align: center">
                            {{ formatISODateToDMY(item.tungay) }}
                          </td>
                          <td style="text-align: center">
                            {{ item.tenphuongthucdong }}
                          </td>
                          <td
                            style="
                              text-align: center;
                              font-weight: 500;
                              color: red;
                            "
                          >
                            {{ formatCurrency(item.sotien) }}
                          </td>
                          <!-- tỉnh-->
                          <td style="text-align: center">
                            {{ item.tentinh }}
                          </td>
                          <!-- quận huyện -->
                          <td style="text-align: center">
                            {{ item.tenquanhuyen }}
                          </td>
                          <!-- xã phường -->
                          <td>
                            {{ item.tenxaphuong }}
                          </td>
                          <!-- tổ thôn -->
                          <td>
                            {{ item.tothon }}
                          </td>
                          <!-- tỉnh bệnh viện -->
                          <td style="text-align: center">
                            {{ item.benhvientinh }}
                          </td>
                          <!-- bệnh viện -->
                          <td>
                            {{ item.tenbenhvien }}
                          </td>
                          <!-- ghi chú -->
                          <td>
                            {{ item.ghichu }}
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>
              <hr class="navbar-divider" />
              <div class="columns">
                <div class="column" style="margin-top: 10px">
                  <div
                    class="field is-grouped is-flex is-justify-content-center"
                  >
                    <div class="control">
                      <button
                        @click="onSave"
                        class="button is-success is-small"
                      >
                        Xác nhận nạp hồ sơ
                      </button>
                    </div>
                    <div class="control">
                      <button
                        @click="cancelImport"
                        class="button is-warning is-light is-small"
                      >
                        Hủy xác nhận
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import company from "@/config.company";
import { mixinDmBhxh } from "../../mixins/mixinDmBhxh";
import createNumberMask from "text-mask-addons/dist/createNumberMask";
const { DateTime } = require("luxon");
const currencyMask = createNumberMask({
  prefix: "",
  allowDecimal: true,
  includeThousandsSeparator: true,
  allowNegative: false,
});
import Swal from "sweetalert2";
import XLSX from "xlsx";
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";
import vSelect from "vue-select";
import "vue-select/dist/vue-select.css";

import jsPDF from "jspdf";
import "~/assets/font/OpenSans-Light-normal";
import "~/assets/font/OpenSans-SemiBold-normal";
import "~/assets/font/OpenSans-Bold-normal";
import "~/assets/font/OpenSans_SemiCondensed-Italic-normal";
import "~/assets/font/OpenSans-ExtraBold-normal";
import "~/assets/font/OpenSans_Condensed-Bold-normal";
import "~/assets/font/OpenSans-Regular-normal";
import "~/assets/font/Times New Roman Bold-normal";
import backgroundImage from "~/assets/images/bhxh.png";
import qrcode from "~/assets/images/QR-BHXH.png";

import num2words from "vn-num2words";

export default {
  name: "calCastAR",
  mixins: [mixinDmBhxh],

  // nhận mã loại hình và tên loại hình từ page kekhai
  props: {
    maloaihinh: String,
    loaihinh: String,
  },
  components: { DatePicker, vSelect },
  data() {
    return {
      isActive: false,
      isActive_nhaphoso: false,
      isActive_xacnhan: false,
      isActive_import: false,
      mask: currencyMask,
      items: [],
      template_items: [],
      selectedOption: "- Chọn phương án -",
      selectedOptionptd: "- Chọn phương thức đóng -",
      selectedOptionHtnt: "- Chọn hình thức nạp tiền -",
      selectedOptionBenhvien: "- Chọn cơ sở khám chữa bệnh -",
      phuongan: [
        {
          maphuongan: "TM",
          tenphuongan: "Tăng mới",
        },
        {
          maphuongan: "ON",
          tenphuongan: "Đóng tiếp",
        },
      ],
      luongcoso: 0,
      tylengansachtw: 0,
      tylenngansachdp: 0,
      hotrokhac: 0,
      matinh: "",
      tentinh: "",
      dmquanhuyen: [],
      dmbenhvien: [],
      checkXaphuongOpen: false, // khóa xã phường khi load form
      form_response_sucess: [],
      form_response_failed: [],
      isLoading: false,

      formKekhai: {
        sohoso: "",
        dotkekhai: "",
        kykekhai: "",
        ngaykekhai: "",
      },

      // phục vụ việc nhập item từ modal
      addedIndex: 0,
      datanhaphosomodal: {},

      // thêm cho việc import dữ liệu kê khai từ file execl
      fileName: "",
      selectedFile: null,
      isRoleSent: false,

      benhvienInfo: null,

      hanthecu: "",

      dulieuInbienlai: [],
      dulieuTravedeinbienlai: [],
      lockButtonXacnhaninbldt: false, // khóa nút xác nhận biên lai khi đã gửi

      // lam chinh quyen 2 cap 13/7/2025
      cq2cap_Tinh: [],
      cq2cap_Huyen : [],
    };
  },

  mounted() {
    this.isRoleSent = this.user.res_sent;
    // console.log(this.isRoleSent);

    this.loadTinh()
  },

  async created() {
    this.$on("danhmucs-loaded", () => {
      // console.log("Tất cả các danh mục đã được tải.");
      // console.log(this.loaihinhtg);
      // console.log(this.dmluongcs);
      // console.log(this.nguoithu);
      // console.log(this.phuongthucdong);
      // console.log(this.doituongdong);
      // console.log(this.dmtylehotro);
      // console.log(this.dmtinhthanhpho);
      if (this.dmluongcs.length > 0) {
        this.luongcoso = this.dmluongcs[0].luongcs;
      }
      if (this.dmtylehotro.length > 0) {
        this.tylengansachtw = this.dmtylehotro[0].tylengansachtw;
        this.tylenngansachdp = this.dmtylehotro[0].tylenngansachdp;
        this.hotrokhac = this.dmtylehotro[0].tylehotrokhac;
        // console.log(this.tylengansachtw, this.tylenngansachdp);
      }
      if (this.phuongthucdong.length > 0) {
        this.phuongthucdong = this.phuongthucdong.filter(
          (item) => item.maloaihinh !== "IS"
        );
      }
    });

    if (this.user) {
      // Kiểm tra xem người dùng đã đăng nhập chưa
      // console.log("Thông tin người dùng:", this.user);
      this.matinh = this.user.matinh;
      this.tentinh = this.user.tentinh;

      const res_quanhuyen = await this.$axios.get(
        `/api/danhmucs/dmquanhuyenwithmatinh?matinh=${this.matinh}`
      );
      this.dmquanhuyen = res_quanhuyen.data;
      const res_benhvien = await this.$axios.get(
        `/api/danhmucs/dmbenhvienwithtinh-${company.benhvien}?matinh=${this.matinh}`
      );
      this.dmbenhvien = res_benhvien.data;
    } else {
      console.log("Người dùng chưa đăng nhập.");
      // Chuyển hướng người dùng đến trang đăng nhập
      this.$router.push("/login");
    }
  },

  computed: {
    user() {
      return this.$store.state.user || {};
    },

    isDisabled_Xaphuong() {
      return this.checkXaphuongOpen == false;
    },

    totalSoTien() {
      if (this.items && this.items.length > 0) {
        return this.items.reduce((acc, item) => {
          // Xóa tất cả dấu phẩy và sau đó chuyển đổi thành số
          const sotienStr = item.sotien.toString().replace(/,/g, ""); // Loại bỏ dấu phẩy
          let numericValue = parseFloat(sotienStr); // Chuyển thành số

          if (isNaN(numericValue)) {
            numericValue = 0; // Xử lý nếu giá trị không hợp lệ
          }

          return acc + numericValue; // Cộng vào tổng
        }, 0);
      }
      return 0; // Trường hợp không có dữ liệu
    },

    customStyles() {
      return {
        dropdown: (base) => ({
          ...base,
          maxHeight: "50px", // Giảm chiều cao tối đa của danh sách
          overflowY: "auto", // Thêm thanh cuộn
        }),
        dropdownMenu: (base) => ({
          ...base,
          direction: "top", // Hướng xổ lên
        }),
      };
    },

    formattedDate: {
      get() {
        // item.tungay dạng DD/MM/YYYY → YYYY-MM-DD
        const [day, month, year] = this.item.tungay.split("/");
        return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
      },
      set(value) {
        // YYYY-MM-DD → DD/MM/YYYY
        const [year, month, day] = value.split("-");
        this.item.tungay = `${day}/${month}/${year}`;
      },
    },
  },

  methods: {
    async findNguoihuong(masobhxh, index) {
      if (masobhxh !== "") {
        const isDuplicate = this.items.some(
          (item, idx) => idx !== index && item.masobhxh === masobhxh
        );
        if (isDuplicate) {
          Swal.fire({
            text: `Mã số ${masobhxh} vừa được đăng ký trong loại hình này xong, vui lòng kiểm tra lại!`,
            icon: "error",
          });
          this.items[index].masobhxh = "";
          return;
        }

        try {
          this.isLoading = true;
          // 1. Trường họp đầu tiên nếu có trong dữ liệu thẻ
          // Đầu tiên AR và BI sẽ tìm trong dữ liệu thẻ -- db dulieuthe.
          // Chạy API dưới đây để tìm trong dlt
          const res = await this.$axios.get(
            `/api/nguoihuong/find-nguoihuong-masobhxh-theodstg-timhanthe?soSoBhxh=${masobhxh}`
          );

          if (res.data.length > 0) {
            // Tìm bản ghi có denNgay lớn nhất
            const latestRecord = res.data.reduce((max, curr) => {
              const currDate = new Date(
                curr.denNgay.split("/").reverse().join("/")
              );
              const maxDate = new Date(
                max.denNgay.split("/").reverse().join("/")
              );
              return currDate > maxDate ? curr : max;
            });

            // Tìm căn cước công dân trong dữ liệu HGD
            const resHGD = await this.$axios.get(
              `/api/nguoihuong/tim-kiem-thong-tin-hgd?soBhxh=${masobhxh}&SO_DDCN_CCCD_BCA=''`
            );
            let soCmnd_hgd = "";
            // console.log(resHGD.data.canhan.SO_DDCN_CCCD_BCA);
            if (resHGD.data.canhan !== null) {
              soCmnd_hgd = resHGD.data.canhan.SO_DDCN_CCCD_BCA;
              // console.log(resHGD);
            }

            this.isLoading = false;

            // Gán bản ghi có denNgay lớn nhất
            const data = latestRecord;
            // console.log(data);

            try {
              this.items[index].hoten = data.hoTen;
              this.items[index].ngaysinh = data.ngaySinh;
              // console.log(typeof data.gioiTinh);
              this.items[index].cccd = soCmnd_hgd;
              if (data.gioiTinh == "1") {
                this.items[index].gioitinh = "Nam";
              } else {
                this.items[index].gioitinh = "Nữ";
              }

              // this.items[index].cccd = data.soCmnd;
              this.items[index].dienthoai = data.soDienThoai;

              // CODE TÌM HẠN THẺ TỪ 05/06/2025
              // gán hạn thẻ cũ lên form
              this.items[index].hanthecu = data.denNgay;
              const denNgayStr = data.denNgay; // vd: "10/10/2024"
              // const denNgayStr = "15/03/2025";

              // Hàm parse định dạng dd/mm/yyyy thành Date
              const parseDate = (str) => {
                const [day, month, year] = str.split("/").map(Number);
                return new Date(year, month - 1, day);
              };

              // Hàm format Date về dd/mm/yyyy
              const formatDate = (date) => {
                const d = String(date.getDate()).padStart(2, "0");
                const m = String(date.getMonth() + 1).padStart(2, "0");
                const y = date.getFullYear();
                return `${d}/${m}/${y}`;
              };

              const today = new Date();
              const denNgay = parseDate(denNgayStr);
              const bienLai = today;

              const millisecondsPerDay = 1000 * 60 * 60 * 24;
              const diffDays = Math.floor(
                (denNgay - today) / millisecondsPerDay
              );

              // console.log(diffDays);

              if (diffDays > 30) {
                Swal.fire({
                  icon: "info",
                  title: "Thẻ vẫn còn hạn",
                  text: `Thẻ hiện còn hiệu lực thêm ${diffDays} ngày. Cân nhắc trước khi gia hạn!`,
                });
              }

              let tuNgay;

              if (denNgay >= today) {
                // Chưa hết hạn → ngày kế tiếp
                const nextDay = new Date(denNgay);
                nextDay.setDate(nextDay.getDate() + 1);
                tuNgay = nextDay;
              } else {
                const daysDiff = (today - denNgay) / (1000 * 60 * 60 * 24);
                if (daysDiff > 90) {
                  // Hết hạn > 3 tháng → sau hôm nay 30 ngày
                  const next30 = new Date();
                  next30.setDate(next30.getDate() + 30);
                  tuNgay = next30;
                } else {
                  // Hết hạn < 3 tháng → dùng ngày biên lai
                  tuNgay = bienLai;
                }
              }

              this.items[index].tungay = formatDate(tuNgay);
              // console.log("🎯 Hạn thẻ từ (tungay):", this.items[index].tungay);

              this.items[index].matinh = data.maTinhLh;
              // CODE MOI. DIA PHUONG 2 CAP. LAY MA TINH TU DL THE
              // đi tìm tên tỉnh
              const res_tinh = await this.$axios.get(
                `/api/danhmucs/hanhchinh2cap-find-tentinh?province_code=${data.maTinhLh}`
              );
              // console.log(res_tinh.data);
              if (res_tinh.data.length > 0) {
                this.items[index].tentinh = `Tỉnh ${res_tinh.data[0].name}`;
                // console.log(this.items[index].tentinh);
              }
              
              //  TÌM VÀ GÁN LẠI TÊN XÃ MỚI 2 CẤP              
              const res_xa = await this.$axios.get(
                `/api/danhmucs/hanhchinh2cap-find-tenxa?old_ward_code=${data.maXaLh}`
              );
              // console.log(res_xa.data)
              if (res_xa.data.length > 0) {
                this.items[index].tenxaphuong = res_xa.data[0].ward_name;
                this.items[index].maxaphuong = res_xa.data[0].ward_code
                // console.log(this.items[index].tenxaphuong);
                // console.log(this.items[index].maxaphuong);
              }

              // GÁN THÔNG TIN HUYỆN CŨ. data.maXaLh là mã xã cũ
              // tìm thông tin quận huyện cũ theo mã xã cũ
              // select * from dm_xaphuong where matinh=42 and maxaphuong=18070
              const res_huyencu = await this.$axios.get(
                `/api/danhmucs/thongtinquanhuyencu?maxaphuong=${data.maXaLh}`
              );
              // console.log(res_huyencu.data)
              this.items[index].maquanhuyen=res_huyencu.data.maquanhuyen
              this.items[index].tenquanhuyen=res_huyencu.data.tenquanhuyen

              this.items[index].tothon = data.diaChiHk;
              this.items[index].benhvientinh = data.maTinhLh;
            } catch (error) {
              console.log(error.message);
            }
          } else {
            // 2. Trường hợp không có trong dữ liệu thẻ thì đi tìm trong DL HGD
            const resHGD = await this.$axios.get(
              `/api/nguoihuong/tim-kiem-thong-tin-hgd?soBhxh=${masobhxh}&SO_DDCN_CCCD_BCA=''`
            );
            // console.log(resHGD);
            if (resHGD.data.canhan !== null) {
              // console.log(resHGD);
              this.isLoading = false;
              const data = resHGD.data.canhan;
              try {
                this.items[index].hoten = data.hoTen;
                this.items[index].ngaysinh = data.ngaySinh;
                // console.log(typeof data.gioiTinh);
                this.items[index].cccd = data.SO_DDCN_CCCD_BCA;
                this.items[index].gioitinh = data.gioiTinh;
                this.items[index].dienthoai = data.soDienThoai;

                if (data.hanThe !== null && data.hanThe !== "") {
                  this.items[index].hanthecu = data.hanThe.split("-")[1]; // Kết quả: "31/12/2025"

                  // this.hanthecu = "31/04/2025"; -- dùng để test
                  // console.log(this.hanthecu);
                  // Hàm parse định dạng dd/mm/yyyy thành Date
                  const parseDate = (str) => {
                    const [day, month, year] = str.split("/").map(Number);
                    return new Date(year, month - 1, day);
                  };

                  // Hàm format Date về dd/mm/yyyy
                  const formatDate = (date) => {
                    const d = String(date.getDate()).padStart(2, "0");
                    const m = String(date.getMonth() + 1).padStart(2, "0");
                    const y = date.getFullYear();
                    return `${d}/${m}/${y}`;
                  };

                  const today = new Date();
                  const denNgay = parseDate(this.items[index].hanthecu);
                  const bienLai = today;

                  // console.log(denNgay);

                  let tuNgay;

                  if (denNgay >= today) {
                    // Chưa hết hạn → ngày kế tiếp
                    const nextDay = new Date(denNgay);
                    nextDay.setDate(nextDay.getDate() + 1);
                    tuNgay = nextDay;
                  } else {
                    const daysDiff = (today - denNgay) / (1000 * 60 * 60 * 24);
                    if (daysDiff > 90) {
                      // Hết hạn > 3 tháng → sau hôm nay 30 ngày
                      const next30 = new Date();
                      next30.setDate(next30.getDate() + 30);
                      tuNgay = next30;
                    } else {
                      // Hết hạn < 3 tháng → dùng ngày biên lai
                      tuNgay = bienLai;
                    }
                  }

                  this.items[index].tungay = formatDate(tuNgay);
                  // console.log("🎯 Hạn thẻ từ (tungay):", this.items[index].tungay);
                } else {
                  this.items[index].hanthecu = "Không tìm thấy hạn thẻ cũ";
                  // Gán ngày hiện tại + 30 ngày
                  const today = new Date();
                  const next30 = new Date();
                  next30.setDate(today.getDate() + 30);

                  const formatDate = (date) => {
                    const d = String(date.getDate()).padStart(2, "0");
                    const m = String(date.getMonth() + 1).padStart(2, "0");
                    const y = date.getFullYear();
                    return `${d}/${m}/${y}`;
                  };

                  this.items[index].tungay = formatDate(next30);
                  // console.log(
                  //   "⚠️ Không có hạn thẻ → gán tungay:",
                  //   this.items[index].tungay
                  // );
                }

                const filename = data.tenFile;
                const parts = filename.split("_");

                const maTinh = parts[4].replace("TTT", "");
                const maHuyen = parts[5].replace("HH", "");
                const maXa = parts[6];

                // console.log("Mã tỉnh:", maTinh); // "42"
                // console.log("Mã huyện:", maHuyen); // "449"
                // console.log("Mã xã:", maXa); // "18754"
                
                // CODE MOI. DIA PHUONG 2 CAP. LAY MA TINH TU DL THE
                // đi tìm tên tỉnh
                const res_tinh = await this.$axios.get(
                  `/api/danhmucs/hanhchinh2cap-find-tentinh?province_code=${maTinh}`
                );
                // console.log(res_tinh.data);
                if (res_tinh.data.length > 0) {
                  this.items[index].tentinh = `Tỉnh ${res_tinh.data[0].name}`;
                  // console.log(this.items[index].tentinh);
                }

                //  TÌM VÀ GÁN LẠI TÊN XÃ MỚI 2 CẤP              
                const res_xa = await this.$axios.get(
                  `/api/danhmucs/hanhchinh2cap-find-tenxa?old_ward_code=${maXa}`
                );
                // console.log(res_xa.data)
                if (res_xa.data.length > 0) {
                  this.items[index].tenxaphuong = res_xa.data[0].ward_name;
                  this.items[index].maxaphuong = res_xa.data[0].ward_code
                  // console.log(this.items[index].tenxaphuong);
                  // console.log(this.items[index].maxaphuong);
                }

                // GÁN THÔNG TIN HUYỆN CŨ. data.maXaLh là mã xã cũ
                // tìm thông tin quận huyện cũ theo mã xã cũ
                // select * from dm_xaphuong where matinh=42 and maxaphuong=18070
                const res_huyencu = await this.$axios.get(
                  `/api/danhmucs/thongtinquanhuyencu?maxaphuong=${maXa}`
                );
                // console.log(res_huyencu.data)
                this.items[index].maquanhuyen=res_huyencu.data.maquanhuyen
                this.items[index].tenquanhuyen=res_huyencu.data.tenquanhuyen


                // this.items[index].matinh = maTinh;
                // // đi tìm tên tỉnh
                // const res_tinh = await this.$axios.get(
                //   `/api/nguoihuong/find-tentinh?matinh=${maTinh}`
                // );
                // if (res_tinh.data.length > 0) {
                //   this.items[index].tentinh = res_tinh.data[0].tentinh;
                //   // console.log(this.items[index].tentinh);
                // }
                // this.items[index].maquanhuyen = maHuyen;
                // // đi tìm tên quận huyện
                // const res_huyen = await this.$axios.get(
                //   `/api/nguoihuong/find-tenhuyen?matinh=${maTinh}&maquanhuyen=${maHuyen}`
                // );
                // if (res_huyen.data.length > 0) {
                //   this.items[index].tenquanhuyen =
                //     res_huyen.data[0].tenquanhuyen;
                //   // console.log(this.items[index].tenquanhuyen);
                // }
                // this.items[index].maxaphuong = maXa;
                // // đi tìm tên xã
                // const res_xa = await this.$axios.get(
                //   `/api/nguoihuong/find-tenxa?matinh=${maTinh}&maquanhuyen=${maHuyen}&maxaphuong=${maXa}`
                // );
                // // console.log(res_xa);

                // if (res_xa.data.length > 0) {
                //   this.items[index].tenxaphuong = res_xa.data[0].tenxaphuong;
                //   // console.log(this.items[index].tenxaphuong);
                // }

                this.items[index].tothon = data.diaChi;
                this.items[index].benhvientinh = maTinh;
              } catch (error) {
                console.log(error.message);
              }

              Swal.fire({
                text: "Không có thông tin cấp thẻ hiện tại của BHXH, đây chỉ là thông tin thẻ hiện đang có trong Hộ gia đình (Hoặc có thể không có). Đề nghị kiểm tra kỹ hồ sơ rồi mới kê khai nhé!",
                // text: "Đã gửi thông tin hồ sơ lên cổng BHXH VN!",
                icon: "success",
              });
            } else {
              Swal.fire({
                text: "Người này hiện không có trong dữ liệu của phần mềm chúng tôi. Bạn hãy tự nhập mới toàn bộ. Hạn thẻ từ ngày sẽ tính sau 30 ngày nữa.",
                icon: "success",
              });

              const today = new Date();
              const next30 = new Date();
              next30.setDate(today.getDate() + 30);
              const d = String(next30.getDate()).padStart(2, "0");
              const m = String(next30.getMonth() + 1).padStart(2, "0");
              const y = next30.getFullYear();
              this.items[index].tungay = `${d}/${m}/${y}`;
              this.items[index].hanthecu = "Không tìm thấy hạn thẻ";
            }
          }
          this.isLoading = false;
        } catch (error) {
          console.log(error);
          this.isLoading = false;
        }
      }
    },

    async findNguoihuong_cccd(cccd, index) {
      if (cccd !== "") {
        const isDuplicate = this.items.some(
          (item, idx) =>
            idx !== index &&
            (item.cccd === cccd || item.cccd === this.items[index].cccd)
        );

        if (isDuplicate) {
          Swal.fire({
            text: `Mã số ${cccd} vừa được đăng ký trong loại hình này xong, vui lòng kiểm tra lại!`,
            icon: "error",
          });

          // Xoá mã số BHXH vừa nhập
          this.items[index].cccd = "";
          return;
        }

        try {
          const res = await this.$axios.get(
            `/api/nguoihuong/find-nguoihuong-cccd-theodstg?soCmnd=${cccd}`
          );
          this.isLoading = true;
          // console.log(res.data);
          if (res.data.length > 0) {
            const resThe = await this.$axios.get(
              `/api/nguoihuong/find-nguoihuong-masobhxh-theodstg-timhanthe?soSoBhxh=${masobhxh}`
            );
            this.isLoading = false;
            const Toast = Swal.mixin({
              toast: true,
              position: "top-end",
              showConfirmButton: false,
              timer: 3000,
              timerProgressBar: true,
              didOpen: (toast) => {
                toast.addEventListener("mouseenter", Swal.stopTimer);
                toast.addEventListener("mouseleave", Swal.resumeTimer);
              },
            });
            Toast.fire({
              icon: "success",
              title:
                "Dữ liệu chỉ mang tính chất tham khảo. Xem và sửa nếu cần thiết !",
            });
            const data = res.data[0];
            const datahanthe = resThe.data[0];
            try {
              this.items[index].hoten = data.hoTen;
              this.items[index].ngaysinh = data.ngaySinh;
              this.items[index].gioitinh = data.gioiTinh;
              this.items[index].cccd = data.soCmnd;
              this.items[index].dienthoai = data.soDienThoai;
              this.items[index].tungay = datahanthe.denNgay;
              this.items[index].matinh = data.maTinh;
              // đi tìm tên tỉnh
              const res_tinh = await this.$axios.get(
                `/api/nguoihuong/find-tentinh?matinh=${data.maTinh}`
              );
              if (res_tinh.data.length > 0) {
                this.items[index].tentinh = res_tinh.data[0].tentinh;
              }
              this.items[index].maquanhuyen = data.maHuyenLh;
              // đi tìm tên quận huyện
              const res_huyen = await this.$axios.get(
                `/api/nguoihuong/find-tenhuyen?matinh=${data.maTinh}&maquanhuyen=${data.maHuyenLh}`
              );
              if (res_huyen.data.length > 0) {
                this.items[index].tenquanhuyen = res_huyen.data[0].tenquanhuyen;
              }
              this.items[index].maxaphuong = data.maXaLh;
              // đi tìm tên xã
              const res_xa = await this.$axios.get(
                `/api/nguoihuong/find-tenxa?matinh=${data.maTinh}&maquanhuyen=${data.maHuyenLh}&maxaphuong=${data.maXaLh}`
              );
              if (res_xa.data.length > 0) {
                this.items[index].tenxaphuong = res_xa.data[0].tenxaphuong;
              }
              this.items[index].tothon = data.diaChiLh;
              this.items[index].benhvientinh = data.maTinh;
              // this.items[index].mabenhvien = data.NoiKhamChuaBenh;
              // đi tìm tên bệnh viện kcb
              // const maBv = `${this.matinh}${data.NoiKhamChuaBenh}`;
              // const res_bv = await this.$axios.get(
              //   `/api/nguoihuong/find-benhvien?mabenhvien=${maBv}`
              // );
              // if (res_bv.data.length > 0) {
              //   this.items[index].tenbenhvien = res_bv.data[0].tenbenhvien;
              // }
            } catch (error) {
              console.log(error.message);
            }
          } else {
            this.isLoading = false;
            const Toast = Swal.mixin({
              toast: true,
              position: "top-end",
              showConfirmButton: false,
              timer: 3000,
              timerProgressBar: true,
              didOpen: (toast) => {
                toast.addEventListener("mouseenter", Swal.stopTimer);
                toast.addEventListener("mouseleave", Swal.resumeTimer);
              },
            });
            Toast.fire({
              icon: "error",
              title: "Không tìm thấy dữ liệu trong kho người hưởng",
            });
            return;
          }
        } catch (error) {
          console.log(error);
        }
      }
    },

    // HANH CHINH 2 CAP
    async loadTinh(){
      const res = await this.$axios.get(
        `/api/danhmucs/hanhchinh2cap-tinh`
      );
      // console.log(res.data);
      if(res.data.length > 0){
        this.cq2cap_Tinh=res.data
      }
          
    },

    async checkItemData(item, index) {
      if (!item.masobhxh) {
        this.$toasted.show("Thiếu mã số BHXH", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!this.isValidMasoBHXH(item.masobhxh)) {
        this.$toasted.show("Mã số BHXH không hợp lệ", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.hoten) {
        this.$toasted.show("Thiếu họ tên", { duration: 3000, theme: "bubble" });
        return false;
      }

      if (!item.ngaysinh) {
        this.$toasted.show("Thiếu ngày sinh", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.gioitinh) {
        this.$toasted.show("Chọn giới tính", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.cccd) {
        this.$toasted.show("Thiếu CCCD", { duration: 3000, theme: "bubble" });
        return false;
      }

      if (!this.isValidCCCD(item.cccd)) {
        this.$toasted.show("CCCD không hợp lệ", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.maphuongan || !item.tenphuongan) {
        this.$toasted.show("Chọn một phương án", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.tungay) {
        this.$toasted.show("Thiếu từ ngày", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.maphuongthucdong || !item.tenphuongthucdong) {
        this.$toasted.show("Thiếu phương thức đóng", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.maquanhuyen || !item.tenquanhuyen) {
        this.$toasted.show("Thiếu quận huyện", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.maxaphuong || !item.tenxaphuong) {
        this.$toasted.show("Thiếu xã phường", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.mabenhvien || !item.tenbenhvien) {
        this.$toasted.show("Chọn bệnh viện", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      if (!item.hinhthucnap) {
        this.$toasted.show("Chọn hình thức nạp tiền", {
          duration: 3000,
          theme: "bubble",
        });
        return false;
      }

      return true;
    },

    async checkItem(item) {
      // console.log(item);
      const index = this.items.findIndex((x) => x === item);
      const isDataValid = await this.checkItemData(item, index);

      if (!isDataValid) return;

      Swal.fire({
        toast: true,
        icon: "success",
        title: "Bản ghi đã đầy đủ dữ liệu!",
        position: "top-end",
        showConfirmButton: false,
        timer: 3000,
        timerProgressBar: true,
      });
    },

    async checkHgd(item) {
      if (item.masobhxh === "") {
        const Toast = Swal.mixin({
          toast: true,
          position: "top-end",
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          didOpen: (toast) => {
            toast.addEventListener("mouseenter", Swal.stopTimer);
            toast.addEventListener("mouseleave", Swal.resumeTimer);
          },
        });
        Toast.fire({
          icon: "success",
          title: `Chưa nhập vào Mã số BHXH`,
        });
      } else {
        try {
          const res = await this.$axios.get(
            `/api/nguoihuong/tim-kiem-thong-tin-hgd?soBhxh=${item.masobhxh}&SO_DDCN_CCCD_BCA=${item.cccd}`
          );

          const ttHgd = res.data.thongtinHgd;
          // console.log(ttHgd);

          Swal.fire({
            html: `
              <div>
                <p><strong style="color: #0d6efd">Thông tin Hộ gia đình</strong></p>
                <p style="font-weight: 800; color: #dc3545; font-size: 15px">
                  Mã hộ: ${ttHgd[0].maHoGiaDinh || ""} | Chủ hộ: ${
              ttHgd[0].chuHo || ""
            }
                </p>
              </div>

              <div style="margin-top: 5px; max-height: 400px; overflow-y: auto;">
                <table border="1" style="border-collapse: collapse; width: 100%; font-size: 13px;">
                  <thead style="background-color: #cfe2ff; font-weight: bold; text-align: center;">
                    <tr>
                      <th>STT HO</th>
                      <th>Họ tên</th>
                      <th>Mã số BHXH</th>
                      <th>Ngày sinh</th>
                      <th>Giới tính</th>
                      <th>Địa chỉ</th>
                      <th>Mối quan hệ với chủ hộ</th>
                      <th>Số CCCD</th>
                      <th>Mã đối tượng</th>
                      <th>Hạn thẻ</th>
                    </tr>
                  </thead>
                  <tbody>
                    ${ttHgd
                      .map(
                        (item) => `
                      <tr style="text-align: center;">
                        <td>${item.stt_ho || ""}</td>
                        <td style="text-align: left;">${item.hoTen || ""}</td>
                        <td>${item.soBhxh || ""}</td>
                        <td>${item.ngaySinh || ""}</td>
                        <td>${item.gioiTinh || ""}</td>
                        <td style="text-align: left;">${item.diaChi || ""}</td>
                        <td>${item.quanHeChuHo || ""}</td>
                        <td>${item.SO_DDCN_CCCD_BCA || ""}</td>
                        <td>${item.maDoiTuongDangTg || ""}</td>
                        <td style="text-align: center;">${
                          item.hanThe || ""
                        }</td>
                      </tr>
                    `
                      )
                      .join("")}
                  </tbody>
                </table>
              </div>
            `,
            width: "90%",
            confirmButtonText: "Đóng",
            customClass: {
              popup: "swal-wide",
            },
          });
        } catch (error) {
          console.log(error);
        }
      }
    },

    addHosokekhai() {
      this.addedIndex = 0; // là chỉ mục index của item hiện tại đang được nhập tại modal
      // Mở trạng thái nhập hồ sơ
      this.selectedOption = "- Chọn phương án -";
      this.selectedOptionBenhvien = "- Chọn cơ sở khám chữa bệnh -";
      this.selectedOptionptd = "- Chọn phương thức đóng -";
      (this.selectedOptionHtnt = "- Chọn hình thức nạp tiền -"),
        (this.isActive_nhaphoso = true);
      // Số lượng phần tử trước khi thêm
      const previousLength = this.items.length;
      // Thêm dòng mới vào mảng
      this.addRow();
      // Số lượng phần tử sau khi thêm
      const currentLength = this.items.length;
      // Nếu số lượng phần tử tăng lên, lấy chỉ số dòng vừa thêm
      this.addedIndex = -1;
      if (currentLength > previousLength) {
        this.addedIndex = currentLength - 1; // Chỉ số dòng mới là phần tử cuối cùng
      }
      // console.log(
      //   "Index của dòng vừa thêm đang được nhập tại modal:",
      //   this.addedIndex
      // );
      this.datanhaphosomodal = this.items[this.addedIndex];
      // console.log(this.datanhaphosomodal);
    },

    // ghi thông tin bệnh viện vào.
    // onBenhVienSelected(selected, index) {
    //   console.log("Selected:", selected);
    //   console.log("Index:", index);

    //   if (selected) {
    //     this.items[index].mabenhvien = selected.mabenhvien;
    //     this.items[index].tenbenhvien = selected.tenbenhvien;
    //     this.items[index].benhvienInfo = selected; // Nếu cần lưu thêm info
    //   }
    // },

    async xacnhanGhihoso() {
      const result = await Swal.fire({
        title: `Xác nhận thêm vào bộ hồ sơ kê khai ?`,
        showDenyButton: true,
        confirmButtonText: "Xác nhận",
        denyButtonText: `Không`,
      });
      if (result.isConfirmed) {
        // check toàn bộ ô để valid form
        const fieldNames = {
          masobhxh: "Mã số BHXH",
          hoten: "Họ tên",
          ngaysinh: "Ngày sinh",
          cccd: "Căn cước công dân",
          maphuongan: "Phương án",
          tungay: "Từ ngày",
          tenphuongthucdong: "Phương thức đóng",
          hinhthucnap: "Hình thức nạp",
          tentinh: "Tỉnh",
          mabenhvien: "Bệnh viện",
        };

        const fieldsToValidate = [
          "masobhxh",
          "hoten",
          "ngaysinh",
          "cccd",
          "maphuongan",
          "tungay",
          "tenphuongthucdong",
          "hinhthucnap",
          "tentinh",
          "mabenhvien",
        ];

        for (const key of fieldsToValidate) {
          if (
            !this.datanhaphosomodal[key] ||
            this.datanhaphosomodal[key] === ""
          ) {
            const fieldName = fieldNames[key] || key;
            const Toast = Swal.mixin({
              toast: true,
              position: "top-end",
              showConfirmButton: false,
              timer: 3000,
              timerProgressBar: true,
              didOpen: (toast) => {
                toast.addEventListener("mouseenter", Swal.stopTimer);
                toast.addEventListener("mouseleave", Swal.resumeTimer);
              },
            });
            Toast.fire({
              icon: "error",
              title: `Vui lòng nhập ${fieldName}!`,
            });
            return;
          }
        }

        this.items[this.addedIndex] = this.datanhaphosomodal;
        this.datanhaphosomodal = {};
        this.isActive_nhaphoso = false;
        // console.log(this.items);
        // console.log(this.benhvienInfo.mabenhvien);
      }
    },

    async cancelNhaphoso() {
      const result = await Swal.fire({
        title: `Xác nhận hủy kê khai hồ sơ ?`,
        showDenyButton: true,
        confirmButtonText: "Xác nhận",
        denyButtonText: `Không`,
      });
      if (result.isConfirmed) {
        this.items.splice(this.addedIndex, 1);
        this.isActive_nhaphoso = false;
      }
    },

    async guiKekhai() {
      // trên data tạo ra 1 mảng dữ liệu để lưu thông tin sau gọi in biên lai
      this.dulieuInbienlai = [];
      if (this.items.length > 0) {
        // console.log(this.items);
        if (this.items.length <= 0) {
          const Toast = Swal.mixin({
            toast: true,
            position: "top-end",
            showConfirmButton: false,
            timer: 3000,
            timerProgressBar: true,
            didOpen: (toast) => {
              toast.addEventListener("mouseenter", Swal.stopTimer);
              toast.addEventListener("mouseleave", Swal.resumeTimer);
            },
          });
          Toast.fire({
            icon: "error",
            title: "Chưa có bản ghi nào !",
          });
          return;
        } else {
          // Kiểm tra dữ liệu trước khi ghi
          const isDataValid = await this.checkFormData();
          if (!isDataValid) {
            // Dừng quá trình lưu dữ liệu nếu dữ liệu không hợp lệ
            return;
          }

          const result = await Swal.fire({
            title: `Xác nhận gửi hồ sơ kê khai ?`,
            showDenyButton: true,
            confirmButtonText: "Xác nhận",
            denyButtonText: `Hủy gửi`,
          });
          if (result.isConfirmed) {
            // console.log(this.items);

            // const current = new Date();
            const nowInVietnam = DateTime.now().setZone("Asia/Ho_Chi_Minh");
            const formattedDate = nowInVietnam.toFormat("dd-MM-yyyy HH:mm:ss");
            const kyKeKhaiFrm = nowInVietnam.toFormat("MM/yyyy");
            // Bắt đầu hiển thị biểu tượng loading
            this.isLoading = true;
            let dataKekhai = [];
            try {
              for (let i = 0; i < this.items.length; i++) {
                this.items[i].sotien = this.items[i].sotien.replace(/,/g, "");
                this.items[i].tienluongcs = this.items[i].tienluongcs.replace(
                  /,/g,
                  ""
                );

                this.items[i].denngay = this.calculateEndDate(
                  this.items[i].tungay,
                  this.items[i].maphuongthucdong
                );

                this.items[i].tennguoitao = this.user.name;
                // ngày biên lai
                // const ngaybienlaiTranform = this.convertDate(
                //   this.items[i].ngaybienlai
                // );
                // this.items[i].ngaybienlai = ngaybienlaiTranform;

                // info add db
                this.items[i].createdAt = formattedDate;
                this.items[i].createdBy = this.user.username;
                this.items[i].updatedAt = "";
                this.items[i].updatedBy = "";

                // thông tin bộ hồ sơ nạp
                this.items[i].nvt_masobhxh = this.user.masobhxh;
                this.items[i].nvt_cccd = this.user.cccd;
                this.items[i].kykekhai = kyKeKhaiFrm;
                this.items[i].ngaykekhai = formattedDate;
                this.items[i].ngaybienlai = formattedDate;

                // đánh dấu hồ sơ đã nạp luôn
                this.items[i].trangthai = 0;

                const uniqueString = this.generateUniqueString();
                this.items[i].hosoIdentity =
                  uniqueString +
                  this.items[i].masobhxh +
                  this.items[i].cccd +
                  this.user.username;

                // **** thêm các thông tin để gửi dữ liệu lên cổng tiếp nhận BHXH VN
                // số tiền, số tháng kiểu float và kiểu int cho từng loại
                // mã tổ chức dịch vụ thu cho công ty an sinh hưng nguyên
                // IS0104S: BHXH tự nguyện
                // IL0001S: Lực lượng tham gia bảo vệ ANTT ở cơ sở
                // BI0099S: BHYT Hộ gia đình
                // AR0099S: BHYT HGĐ làm nông lâm ngư Nghiệp
                // Vậy đối với AR thì mã TCDV thu là: AR0099S

                // mã tổ chức dịch vụ thu cho công ty An sinh 159
                // BI0214M	Tổ chức dịch vụ thu BHYT HGĐ - Công ty TNHH An Sinh 159
                // AR0212M	Tổ chức dịch vụ thu BHYT HGĐ có MSTB - Công ty TNHH An Sinh 159
                // IS0212M	Tổ chức dịch vụ thu BHXH TN - Công ty TNHH An Sinh 159

                let maToChucDvt = "AR0212M";
                let soTien = this.items[i].sotien;
                let soThang = this.items[i].maphuongthucdong;
                let maNhanVienThu = "NVT" + this.items[i].nvt_cccd;
                let tenNhanVienThu = this.user.name;
                let maCqBhxh = this.user.macqbhxh;
                let tenCqBhxh = this.user.tencqbhxh;
                let key = "0123"; // do bhxh vn cung cấp
                let tuNgay = this.items[i].tungay;
                let denNgay = this.calculateEndDate(tuNgay, soThang);

                // thông tin biên lai
                const currentYear = new Date().getFullYear();

                // Loại bỏ dữ liệu không cần thiết bằng destructuring
                const {
                  info_benhvien,
                  info_huyen,
                  info_phuongan,
                  info_tinh,
                  info_xaphuong,
                  phuongthucdong,
                  ...filteredItem
                } = this.items[i];

                // Thêm vào mảng mới
                // Tạo một đối tượng chứa các phần khai báo mới
                const additionalData = {
                  maToChucDvt,
                  soTien,
                  soThang,
                  maNhanVienThu,
                  tenNhanVienThu,
                  maCqBhxh,
                  tenCqBhxh,
                  key,
                  tuNgay,
                  denNgay,
                  currentYear, // thêm năm hiện tại vào
                };

                // Thêm cả filteredItem và additionalData vào mảng dataKekhai
                dataKekhai.push({
                  ...filteredItem,
                  ...additionalData,
                });

                const tiendong = parseInt(
                  this.items[i].sotien.replace(/,/g, ""),
                  10
                );

                // lấy tên biên lai để lưu
                const formattedForFilename = formattedDate.replace(
                  /[-: ]/g,
                  "_"
                );
                const urlNameInvoice = `${this.items[i].hosoIdentity}_${formattedForFilename}_${this.items[i].hoten}`;

                // console.log(urlNameInvoice);

                const dataPost = {
                  hosoIdentity: this.items[i].hosoIdentity,
                  maSoBhxh: this.items[i].masobhxh,
                  hoTen: this.items[i].hoten,
                  soCccd: this.items[i].cccd,
                  ngaySinh: this.items[i].ngaysinh,
                  gioiTinh: this.items[i].gioitinh,
                  soDienThoai: this.items[i].dienthoai,
                  nguoithutien: this.items[i].tennguoitao,
                  loaiDt: this.items[i].tenloaihinh,
                  soTien: tiendong,
                  soThang: this.items[i].maphuongthucdong,
                  tuNgay: this.items[i].tungay,
                  denNgay: this.items[i].denngay,
                  tuThang: this.items[i].tuthang,
                  denThang: this.items[i].denthang,
                  maDaiLy: this.items[i].madaily,
                  tenDaiLy: this.items[i].tendaily,
                  createdBy: this.user.username,
                  sobienlai: "",
                  ngaybienlai: formattedDate,
                  maloaihinh: this.items[i].maloaihinh,
                  tothon: this.items[i].tothon,
                  tenquanhuyen: this.items[i].tenquanhuyen,
                  tentinh: this.items[i].tentinh,
                  currentYear: currentYear,
                  urlNameInvoice: urlNameInvoice,
                };

                this.dulieuInbienlai.push(dataPost);
              }

              // console.log(dataKekhai);

              const result = await this.$axios.post(
                `/api/kekhai/add-kekhai-series`,
                dataKekhai
              );

              if (result.status === 200) {
                this.isLoading = false;
                // Đợi 300ms trước khi gọi tiếp (đảm bảo DB đã xử lý xong)
                await new Promise((resolve) => setTimeout(resolve, 300));

                const hosoIds = this.dulieuInbienlai.map(
                  (item) => item.hosoIdentity
                );

                const rsIdtity = await this.$axios.post(
                  `/api/kekhai/getdskekhaiwithhsidentity`,
                  hosoIds
                );

                // console.log(rsIdtity.data);
                this.dulieuTravedeinbienlai = rsIdtity.data;
                this.isActive_xacnhan = true;
              }
            } catch (error) {
              // console.log(error);
              this.isLoading = false;
            }
          }
        }
      } else {
        const Toast = Swal.mixin({
          toast: true,
          position: "top-end",
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          didOpen: (toast) => {
            toast.addEventListener("mouseenter", Swal.stopTimer);
            toast.addEventListener("mouseleave", Swal.resumeTimer);
          },
        });
        Toast.fire({
          icon: "error",
          title: "Không có hồ sơ nào để xác nhận",
        });
      }
    },

    async addRow() {
      this.lockButtonXacnhaninbldt = false;
      const phuongAnMacDinh = this.phuongan.find(
        (p) => p.maphuongan === "ON"
      ) || { maphuongan: "", tenphuongan: "" };

      const phuongThucMacDinh = this.phuongthucdong.find(
        (p) => p.maphuongthuc === "12"
      ) || { maphuongthuc: "", tenphuongthuc: "" };

      // load xã theo tỉnh 
      const response = await this.$axios.get(
          `/api/danhmucs/hanhchinh2cap-xa-with-ma-tinh?province_code=${this.matinh}`
        );
      const dataXa = response.data
      try {
        this.items.push({
          matochuc: this.user.matochuc,
          tentochuc: this.user.tentochuc,
          madaily: this.user.madaily,
          tendaily: this.user.tendaily,
          // loại hình nhận từ props kekhai
          maloaihinh: this.maloaihinh,
          tenloaihinh: this.loaihinh,
          // info human
          masobhxh: "",
          hoten: "",
          ngaysinh: "",
          gioitinh: "",
          cccd: "",
          dienthoai: "",
          // ke khai tham gia
          info_phuongan: this.phuongan,
          maphuongan: phuongAnMacDinh.maphuongan,
          tenphuongan: phuongAnMacDinh.tenphuongan,
          tienluongcs: this.luongcoso,
          tylensnnht: this.tylengansachtw,
          tylensdp: this.tylenngansachdp,
          hotrokhac: this.hotrokhac,
          tungay: "",
          denngay: "",
          phuongthucdong: this.phuongthucdong,
          maphuongthucdong: phuongThucMacDinh.maphuongthuc,
          tenphuongthucdong: phuongThucMacDinh.tenphuongthuc,
          sotien: 0, // tiền phải đóng
          info_tinh: { matinh: this.matinh, tentinh: this.tentinh }, // tỉnh mặc định sẽ load theo tên người dùng login
          matinh: this.matinh,
          tentinh: this.tentinh,
          info_huyen: this.dmquanhuyen,
          maquanhuyen: "",
          tenquanhuyen: "",
          info_xaphuong: dataXa,
          maxaphuong: "",
          tenxaphuong: "",
          tothon: "",
          info_benhvien: this.dmbenhvien,
          benhvientinh: this.matinh,
          mabenhvien: "",
          tenbenhvien: "",
          ghichu: "",
          // phải kê vào để lưu CSDL những cái này không có trong loại hình này
          madoituong: "",
          tendoituong: "",
          tuthang: "",
          denthang: "",
          nguoithu: "",
          manguoithu: 0,
          tylengansachdiaphuong: 0,
          tyledong: 0,
          muctiendong: 0,
          tientunguyendong: 0,
          tienlai: 0,
          tiennsnnht: 0,
          tiennsdp: 0,

          // hồ sơ kê khai
          dotkekhai: "",
          kykekhai: "",
          ngaykekhai: "",
          ngaybienlai: "",
          sobienlai: "",
          trangthai: 0,

          status_hosoloi: 0,
          status_naptien: 0,
          hinhthucnap: 1,
          hanthecu: "",
        });

        // Tính tiền ngay sau khi add
        this.$nextTick(() => {
          const index = this.items.length - 1;
          this.tinhTienPhaiDong(index);
        });

        // console.log(this.items)
      } catch (error) {
        console.log(error);
      }
    },

    deleteRow(index) {
      this.items.splice(index, 1);
    },

    copyRow() {
      try {
        let newRow = {}; // Tạo một đối tượng mới để lưu trữ dòng mới

        // Nếu có ít nhất một dòng trong items
        if (this.items.length > 0) {
          // Sao chép dữ liệu của dòng trước đó
          const previousRow = this.items[this.items.length - 1];
          Object.assign(newRow, previousRow);
        }

        // Thêm dòng mới vào mảng items
        this.items.push(newRow);
      } catch (error) {
        console.error("Error adding row:", error);
      }
    },

    formatCurrency(number) {
      return number.toLocaleString("vi-VN", {
        style: "currency",
        currency: "VND",
      });
    },

    formatISODateToDMY(isoDateString) {
      const dateFormat = this.identifyDateFormat(isoDateString);
      if (dateFormat == "YYYY-MM-DD") {
        const date = new Date(isoDateString);

        // Lấy ngày, tháng và năm từ đối tượng Date
        const day = date.getDate().toString().padStart(2, "0");
        const month = (date.getMonth() + 1).toString().padStart(2, "0");
        const year = date.getFullYear();

        // Tạo chuỗi ngày tháng dd/mm/yyyy
        const formattedDate = `${day}/${month}/${year}`;

        return formattedDate;
      } else {
        return isoDateString;
      }
    },

    // phương án
    async phuonganChange(e, index) {
      const maphuongan = e.target.value;
      const tenphuongan = e.target.options[e.target.selectedIndex].text;
      this.items[index].maphuongan = maphuongan;
      this.items[index].tenphuongan = tenphuongan;
    },

    tinhTienPhaiDong(index) {
      const item = this.items[index];
      const thangdong = parseInt(item.maphuongthucdong); // số tháng đóng
      const luongcs = this.luongcoso;
      const tyleTW = this.tylengansachtw;
      const tyleDP = this.tylenngansachdp;
      const tyleKhac = parseFloat(item.hotrokhac || 0); // nếu chưa nhập mặc định 0

      // Tổng tiền phải đóng (chưa trừ hỗ trợ)
      const tongTien = luongcs * 0.045 * thangdong;

      // Hỗ trợ trung ương
      const tienTWHotro = tongTien * (tyleTW / 100);

      // Hỗ trợ địa phương
      const tienDPHotro = tongTien * (tyleDP / 100);

      // Hỗ trợ khác
      const tienKhac = tongTien * (tyleKhac / 100);

      // Tổng tiền phải đóng sau hỗ trợ
      const soTienThucDong = tongTien - (tienTWHotro + tienDPHotro + tienKhac);

      // Gán lại vào item
      item.tiennsnnht = tienTWHotro;
      item.tiennsdp = tienDPHotro;
      item.sotien = soTienThucDong;
      item.tylensnnht = tyleTW;
      item.tylensdp = tyleDP;
    },

    async phuongthucdChange(e, index) {
      const maphuongthucdong = e.target.value;
      const tenphuongthucdong = e.target.options[e.target.selectedIndex].text;

      this.items[index].maphuongthucdong = maphuongthucdong;
      this.items[index].tenphuongthucdong = tenphuongthucdong;

      // Gọi lại hàm tính tiền
      this.tinhTienPhaiDong(index);
    },

    // tỉnh thành phố
    async provinceChange(e, index) {
      // lấy thông tin thay đổi từ người dùng select
      const matinh = e.target.value;
      const tentinh = e.target.options[e.target.selectedIndex].text;
      
      // lấy dữ liệu quận huyện từ mã tỉnh đã được chọn
      try {
        this.isLoading = true
        const response = await this.$axios.get(
          `/api/danhmucs/hanhchinh2cap-xa-with-ma-tinh?province_code=${matinh}`
        );
        // console.log(response.data);
        
        // bind dữ liệu vào dữ liệu select của items để cho từng item sử dụng
        if(response.data.length > 0){
          this.checkXaphuongOpen = true;
          this.items[index].info_xaphuong = response.data;
          this.items[index].matinh = matinh;
          this.items[index].tentinh = tentinh;
        }
        this.isLoading=false
      } catch (error) {
        this.isLoading=false
        console.error("Error fetching data:", error);
      }
    },

    // quận huyện
    async quanhuyenChange(e, index) {
      const maquanhuyen = e.target.value;
      const tenquanhuyen = e.target.options[e.target.selectedIndex].text;
      this.items[index].maquanhuyen = maquanhuyen;
      this.items[index].tenquanhuyen = tenquanhuyen;
      try {
        const response = await this.$axios.get(
          `/api/danhmucs/dmxaphuongwithmahuyen?maquanhuyen=${maquanhuyen}`
        );
        if (response.data.length > 0) {
          this.items[index].info_xaphuong = response.data;
          this.checkXaphuongOpen = true;
        }
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },

    // xã phường
    async xaphuongChange(ward_code, index) {
      const item = this.items[index];

      // Tìm object bệnh viện trong danh sách
      const selected = item.info_xaphuong.find(
        b => b.ward_code === ward_code
      );

      // Gán mã và tên bệnh viện
      item.maxaphuong = ward_code;
      item.tenxaphuong = selected ? selected.ward_name : "";

      // console.log(this.items)
    },

    // tỉnh bệnh viện
    async benhvienChange(e, index) {
      const matinh = e.target.value;
      this.items[index].benhvientinh = matinh;
      try {
        const response = await this.$axios.get(
          `/api/danhmucs/dmbenhvienwithtinh?matinh=${matinh}`
        );
        if (response.data.length > 0) {
          this.items[index].info_benhvien = response.data;
          this.checkXaphuongOpen = true;
        }
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },

    // thông tin bệnh viện
    async hopChange(e, index) {
      const mabenhvien = e.target.value;
      const text = e.target.options[e.target.selectedIndex].text;

      // Tách ra
      const parts = text.split(" - ");
      const tenbenhvien = parts[1] ? parts[1].trim() : "";

      this.items[index].mabenhvien = mabenhvien;
      this.items[index].tenbenhvien = tenbenhvien;
      // console.log(this.items[index]);
    },

    async hopChangeReset(mabenhvien, index) {
      const item = this.items[index];

      // Tìm object bệnh viện trong danh sách
      const selected = item.info_benhvien.find(
        b => b.mabenhvien === mabenhvien
      );

      // Gán mã và tên bệnh viện
      item.mabenhvien = mabenhvien;
      item.tenbenhvien = selected ? selected.tenbenhvien : "";
    },


    hinhthucNap(event, index) {
      const selectedOption = event.target.value;
      // console.log(selectedOption);
      if (selectedOption) {
        this.items[index].hinhthucnap = selectedOption;
        this.datanhaphosomodal.hinhthucnap = selectedOption;
      }
    },

    // check mã số bhxh
    isValidMasoBHXH(masobhxh) {
      // Loại bỏ tất cả các ký tự không phải số
      const cleanedCCCD = masobhxh.replace(/\D/g, "");
      // Kiểm tra độ dài của CCCD và số đầu tiên
      return cleanedCCCD.length === 10;
    },

    // check phone number
    // isValidPhoneNumber(phoneNumber) {
    //   const cleanedPhoneNumber = phoneNumber.replace(/\D/g, "");
    //   return cleanedPhoneNumber.length === 10;
    // },

    // check số cccd
    isValidCCCD(cccd) {
      // Loại bỏ tất cả các ký tự không phải số
      const cleanedCCCD = cccd.replace(/\D/g, "");
      // Kiểm tra độ dài của CCCD và số đầu tiên
      return cleanedCCCD.length === 12 && cleanedCCCD.charAt(0) === "0";
    },

    // design at 20h30 03/05/2024
    // check số biên lai
    isValidSobienlai(sobienlai) {
      // Loại bỏ tất cả các ký tự không phải số
      const cleanedSobienlai = sobienlai.replace(/\D/g, "");
      // Kiểm tra độ dài của số biên lai
      return cleanedSobienlai.length === 7;
    },

    checkSobienlai(sobienlai) {
      if (sobienlai !== "") {
        // console.log(sobienlai.length);
        if (sobienlai.length !== 7) {
          const Toast = Swal.mixin({
            toast: true,
            position: "top-end",
            showConfirmButton: false,
            timer: 3000,
            timerProgressBar: true,
            didOpen: (toast) => {
              toast.addEventListener("mouseenter", Swal.stopTimer);
              toast.addEventListener("mouseleave", Swal.resumeTimer);
            },
          });
          Toast.fire({
            icon: "error",
            title: "Số biên lai phải có đúng 7 chữ số !",
          });
        }
      }
    },

    checkNowDateNgaybienlai(ngaybienlai) {
      const nowInVietnam = DateTime.now().setZone("Asia/Ho_Chi_Minh");
      const dateNow = nowInVietnam.toFormat("yyyy-MM-dd");
      // console.log(dateNow);
      const month = nowInVietnam.month; // Tháng hiện tại
      const year = nowInVietnam.year; // năm hiện tại
      const monthYearNow = month + year;
      // check tháng ngày năm của ngày biên lại nhập vào
      const dateNgaybienlai = new Date(ngaybienlai);
      // Lấy giá trị tháng (tháng bắt đầu từ 0, cần cộng 1)
      const monthBienlai = dateNgaybienlai.getMonth() + 1;
      const yearBienlai = dateNgaybienlai.getFullYear();
      const monthYearBienlai = monthBienlai + yearBienlai;
      // console.log(monthBienlai);
      // console.log(month);
      if (monthYearBienlai !== monthYearNow) {
        const Toast = Swal.mixin({
          toast: true,
          position: "top-end",
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          didOpen: (toast) => {
            toast.addEventListener("mouseenter", Swal.stopTimer);
            toast.addEventListener("mouseleave", Swal.resumeTimer);
          },
        });
        Toast.fire({
          icon: "error",
          title:
            "Tháng năm biên lai bạn nhập có tháng năm không phải tháng năm tại thời điểm hiện tại",
        });
      }
    },

    checkAlertCccd(cccd) {
      if (cccd !== "") {
        const cleanedCCCD = cccd.replace(/\D/g, "");
        if (cleanedCCCD.length !== 12) {
          const Toast = Swal.mixin({
            toast: true,
            position: "top-end",
            showConfirmButton: false,
            timer: 3000,
            timerProgressBar: true,
            didOpen: (toast) => {
              toast.addEventListener("mouseenter", Swal.stopTimer);
              toast.addEventListener("mouseleave", Swal.resumeTimer);
            },
          });
          Toast.fire({
            icon: "error",
            title: "CCCD phải có đúng 12 chữ số !",
          });
        }
      }
    },

    // checkAlertSodienthoai(dienthoai) {
    //   if (dienthoai !== "") {
    //     const cleanedPhone = dienthoai.replace(/\D/g, "");
    //     if (cleanedPhone.length !== 10) {
    //       const Toast = Swal.mixin({
    //         toast: true,
    //         position: "top-end",
    //         showConfirmButton: false,
    //         timer: 3000,
    //         timerProgressBar: true,
    //         didOpen: (toast) => {
    //           toast.addEventListener("mouseenter", Swal.stopTimer);
    //           toast.addEventListener("mouseleave", Swal.resumeTimer);
    //         },
    //       });
    //       Toast.fire({
    //         icon: "error",
    //         title: "Số điện thoại phải có đúng 10 chữ số !",
    //       });
    //     }
    //   }
    // },

    async checkFormData() {
      for (let i = 0; i < this.items.length; i++) {
        if (!this.items[i].masobhxh) {
          this.$toasted.show("Thiếu mã số BHXH", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.isValidMasoBHXH(this.items[i].masobhxh)) {
          this.$toasted.show("Mã số BHXH không hợp lệ", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.items[i].hoten) {
          this.$toasted.show("Thiếu họ tên", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (this.items[i].ngaysinh == "") {
          this.$toasted.show("Thiếu ngày sinh", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.items[i].gioitinh) {
          this.$toasted.show("Chọn giới tính", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.items[i].cccd) {
          this.$toasted.show("Thiếu CCCD", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.isValidCCCD(this.items[i].cccd)) {
          this.$toasted.show("CCD không hợp lệ", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        

        if (!this.items[i].maphuongan || !this.items[i].tenphuongan) {
          this.$toasted.show("Chọn một phương án", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.items[i].tungay) {
          this.$toasted.show("Thiếu từ ngày", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (
          !this.items[i].maphuongthucdong ||
          !this.items[i].tenphuongthucdong
        ) {
          this.$toasted.show("Thiếu phương thức đóng", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.items[i].maxaphuong || !this.items[i].tenxaphuong) {
          this.$toasted.show("Thiếu xã phường", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }


        if (!this.items[i].mabenhvien || !this.items[i].tenbenhvien) {
          this.$toasted.show("Chọn bệnh viện", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }

        if (!this.items[i].hinhthucnap) {
          this.$toasted.show("Chọn hình thức nạp tiền", {
            duration: 3000,
            theme: "bubble",
          });

          return false;
        }
      }
      // Nếu tất cả thông tin đều hợp lệ, trả về true để cho phép quá trình lưu dữ liệu
      return true;
    },

    // chuyển đổi định dạng ngày tháng
    convertDate(inputDate) {
      const [year, month, day] = inputDate.split("-");
      return `${day}/${month}/${year}`;
    },

    identifyDateFormat(dateString) {
      // Biểu thức chính quy cho định dạng YYYY-MM-DD
      const regexYYYYMMDD = /^\d{4}-\d{2}-\d{2}$/;

      // Biểu thức chính quy cho định dạng DD/MM/YYYY
      const regexDDMMYYYY = /^\d{2}\/\d{2}\/\d{4}$/;

      // Kiểm tra xem chuỗi ngày tháng thuộc định dạng nào
      if (regexYYYYMMDD.test(dateString)) {
        return "YYYY-MM-DD";
      } else if (regexDDMMYYYY.test(dateString)) {
        return "DD/MM/YYYY";
      } else {
        return "UNKNOWN";
      }
    },

    calculateEndDate(tuNgay, soThang) {
      // Chuyển đổi tuNgay từ chuỗi "dd/mm/yyyy" sang đối tượng Date
      const [day, month, year] = tuNgay.split("/").map(Number);
      let startDate = new Date(year, month - 1, day); // Month in Date is 0-based

      // Cộng thêm số tháng vào ngày bắt đầu
      startDate.setMonth(startDate.getMonth() + Number(soThang));

      // Trừ một ngày để có ngày cuối cùng của tháng trước tháng kết thúc
      startDate.setDate(startDate.getDate() - 1);

      // Lấy ngày, tháng, năm của ngày kết thúc
      let endDay = String(startDate.getDate()).padStart(2, "0");
      let endMonth = String(startDate.getMonth() + 1).padStart(2, "0"); // Month is 0-based, so add 1
      let endYear = startDate.getFullYear();

      // Trả về ngày kết thúc dưới dạng "dd/mm/yyyy"
      return `${endDay}/${endMonth}/${endYear}`;
    },

    generateUniqueString() {
      return `${Date.now()}-${Math.random().toString(36).substring(2, 15)}`;
    },

    async xacnhanThoat() {
      const result = await Swal.fire({
        title: `Xác nhận thoát?`,
        showDenyButton: true,
        confirmButtonText: "Xác nhận",
        denyButtonText: `Hủy thoát`,
      });
      if (result.isConfirmed) {
        this.isActive = false;
      }
    },

    hoanTatDongHs() {
      // console.log("hoantatdong");
      this.items = [];
      this.dulieuTravedeinbienlai = [];
      this.dulieuInbienlai = [];
      this.isActive_xacnhan = false;
    },

    async onSave() {
      // đoạn này theo code mới là sẽ bấm để lưu dữ liệu biên lai
      // Kiểm tra dữ liệu trước khi ghi
      // console.log(this.dulieuTravedeinbienlai);
      const isDataValid = await this.checkFormData();
      if (!isDataValid) {
        // Dừng quá trình lưu dữ liệu nếu dữ liệu không hợp lệ
        return;
      }

      const result = await Swal.fire({
        title: `Xác nhận biên lai điện tử?`,
        showDenyButton: true,
        confirmButtonText: "Xác nhận",
        denyButtonText: `Hủy gửi`,
      });
      if (result.isConfirmed) {
        // console.log(this.items);

        // const current = new Date();
        const nowInVietnam = DateTime.now().setZone("Asia/Ho_Chi_Minh");
        const formattedDate = nowInVietnam.toFormat("dd-MM-yyyy HH:mm:ss");
        try {
          // Bắt đầu hiển thị biểu tượng loading
          this.isLoading = true;
          // thông tin biên lai
          const currentYear = new Date().getFullYear();

          // lấy tên biên lai để lưu
          for (let i = 0; i < this.dulieuTravedeinbienlai.length; i++) {
            const item = this.dulieuTravedeinbienlai[i];

            const formattedForFilename = formattedDate.replace(/[-: ]/g, "_");
            const urlNameInvoice = `${item.hosoIdentity}_${formattedForFilename}_${item.sobienlai}_${item.hoten}`;
            const cccd_nguoithutien = item.sohoso.slice(-12);

            const dataPost = {
              hosoIdentity: item.hosoIdentity,
              maSoBhxh: item.masobhxh,
              hoTen: item.hoten,
              soCccd: item.cccd,
              ngaySinh: item.ngaysinh,
              gioiTinh: item.gioitinh,
              soDienThoai: item.dienthoai,
              nguoithutien: item.tennguoitao,
              loaiDt: item.tenloaihinh,
              soTien: item.sotien,
              soThang: item.maphuongthucdong,
              tuNgay: item.tungay,
              denNgay: item.denngay,
              tuThang: item.tuthang,
              denThang: item.denthang,
              maDaiLy: item.madaily,
              tenDaiLy: item.tendaily,
              createdBy: this.user.username,
              sobienlai: item.sobienlai,
              ngaybienlai: formattedDate,
              maloaihinh: item.maloaihinh,
              tothon: item.tothon,
              tenquanhuyen: item.tenquanhuyen,
              tentinh: item.tentinh,
              currentYear: currentYear,
              urlNameInvoice: urlNameInvoice,
              maphuongan: item.maphuongan,
              cccd_nguoithutien: cccd_nguoithutien,
            };

            const ghibienlai = await this.$axios.post(
              `/api/kekhai/ghidulieubienlai`,
              dataPost
            );

            // lưu biên lai vào máy chủ
            await this.inBienLaiDientu(dataPost);
            // console.log("xongbienlai");
          }

          const hosoIds = this.dulieuTravedeinbienlai.map(
            (item) => item.hosoIdentity
          );

          const rsIdtity = await this.$axios.post(
            `/api/kekhai/getdskekhaiwithhsidentity`,
            hosoIds
          );
          this.dulieuTravedeinbienlai = rsIdtity.data;
          this.isLoading = false;
          this.lockButtonXacnhaninbldt = true; // khoá nút xác nhận biên lai điện tử

          Swal.fire({
            title: "Hoàn tất xác nhận toàn bộ hồ sơ!",
            icon: "success",
          });
          // console.log("check hàm xem biên lai:", this.xemBienLai);
        } catch (error) {
          // console.log(error);
          this.isLoading = false;
        }
      }
    },

    async xemBienLai(item) {
      // console.log(item.hosoIdentity);

      try {
        const res = await this.$axios.get(
          `/api/kekhai/view-item-bienlai?hosoIdentity=${item.hosoIdentity}`
        );

        // console.log(res);

        const hs = res.data.hs;
        if (hs && hs.urlNameInvoice) {
          const fileName = `${hs.sobienlai}_${encodeURIComponent(
            hs.hoten
          )}.pdf`;
          const pdfUrl = `${company.clientURL}/bienlaidientu/${hs.urlNameInvoice}.pdf`;
          // const pdfUrl = `http://localhost:1970/bienlaidientu/${hs.urlNameInvoice}.pdf`;
          // console.log(pdfUrl);

          window.open(pdfUrl, "_blank");
        } else {
          console.warn("Thiếu thông tin số biên lai hoặc họ tên!");
          this.$swal.fire({
            icon: "error",
            title: "Lỗi",
            text: "Không lấy được thông tin biên lai.",
          });
        }
      } catch (error) {
        console.error("Lỗi khi gọi API:", error);
        this.$swal.fire({
          icon: "error",
          title: "Lỗi",
          text: "Không thể kết nối đến máy chủ.",
        });
      }
    },

    async onSave1() {
      const matochuc = this.user.matochuc;

      if (this.items.length <= 0) {
        const Toast = Swal.mixin({
          toast: true,
          position: "top-end",
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          didOpen: (toast) => {
            toast.addEventListener("mouseenter", Swal.stopTimer);
            toast.addEventListener("mouseleave", Swal.resumeTimer);
          },
        });
        Toast.fire({
          icon: "error",
          title: "Chưa có bản ghi nào !",
        });
        return;
      } else {
        // Kiểm tra dữ liệu trước khi ghi
        const isDataValid = await this.checkFormData();
        if (!isDataValid) {
          // Dừng quá trình lưu dữ liệu nếu dữ liệu không hợp lệ
          return;
        }

        const result = await Swal.fire({
          title: `Xác nhận gửi hồ sơ kê khai ?`,
          showDenyButton: true,
          confirmButtonText: "Xác nhận",
          denyButtonText: `Hủy gửi`,
        });
        if (result.isConfirmed) {
          // console.log(this.items);

          // const current = new Date();
          const nowInVietnam = DateTime.now().setZone("Asia/Ho_Chi_Minh");
          const formattedDate = nowInVietnam.toFormat("dd-MM-yyyy HH:mm:ss");
          const kyKeKhaiFrm = nowInVietnam.toFormat("MM/yyyy");
          // Bắt đầu hiển thị biểu tượng loading
          this.isLoading = true;
          let dataKekhai = [];
          try {
            for (let i = 0; i < this.items.length; i++) {
              this.items[i].sotien = this.items[i].sotien.replace(/,/g, "");
              this.items[i].tienluongcs = this.items[i].tienluongcs.replace(
                /,/g,
                ""
              );

              this.items[i].denngay = this.calculateEndDate(
                this.items[i].tungay,
                this.items[i].maphuongthucdong
              );

              this.items[i].tennguoitao = this.user.name;
              // ngày biên lai
              // const ngaybienlaiTranform = this.convertDate(
              //   this.items[i].ngaybienlai
              // );
              // this.items[i].ngaybienlai = ngaybienlaiTranform;

              // info add db
              this.items[i].createdAt = formattedDate;
              this.items[i].createdBy = this.user.username;
              this.items[i].updatedAt = "";
              this.items[i].updatedBy = "";

              // thông tin bộ hồ sơ nạp
              this.items[i].nvt_masobhxh = this.user.masobhxh;
              this.items[i].nvt_cccd = this.user.cccd;
              this.items[i].kykekhai = kyKeKhaiFrm;
              this.items[i].ngaykekhai = formattedDate;

              // đánh dấu hồ sơ đã nạp luôn
              this.items[i].trangthai = 0;

              const uniqueString = this.generateUniqueString();
              this.items[i].hosoIdentity =
                uniqueString +
                this.items[i].masobhxh +
                this.items[i].cccd +
                this.user.username;

              // **** thêm các thông tin để gửi dữ liệu lên cổng tiếp nhận BHXH VN
              // số tiền, số tháng kiểu float và kiểu int cho từng loại
              // mã tổ chức dịch vụ thu cho công ty an sinh hưng nguyên
              // IS0104S: BHXH tự nguyện
              // IL0001S: Lực lượng tham gia bảo vệ ANTT ở cơ sở
              // BI0099S: BHYT Hộ gia đình
              // AR0099S: BHYT HGĐ làm nông lâm ngư Nghiệp
              // Vậy đối với AR thì mã TCDV thu là: AR0099S

              // mã tổ chức dịch vụ thu cho công ty An sinh 159
              // BI0214M	Tổ chức dịch vụ thu BHYT HGĐ - Công ty TNHH An Sinh 159
              // AR0212M	Tổ chức dịch vụ thu BHYT HGĐ có MSTB - Công ty TNHH An Sinh 159
              // IS0212M	Tổ chức dịch vụ thu BHXH TN - Công ty TNHH An Sinh 159

              let maToChucDvt = "AR0212M";
              let soTien = this.items[i].sotien;
              let soThang = this.items[i].maphuongthucdong;
              let maNhanVienThu = "NVT" + this.items[i].nvt_cccd;
              let tenNhanVienThu = this.user.name;
              let maCqBhxh = this.user.macqbhxh;
              let tenCqBhxh = this.user.tencqbhxh;
              let key = "0123"; // do bhxh vn cung cấp
              let tuNgay = this.items[i].tungay;
              let denNgay = this.calculateEndDate(tuNgay, soThang);

              // Loại bỏ dữ liệu không cần thiết bằng destructuring
              const {
                info_benhvien,
                info_huyen,
                info_phuongan,
                info_tinh,
                info_xaphuong,
                phuongthucdong,
                ...filteredItem
              } = this.items[i];

              // Thêm vào mảng mới
              // Tạo một đối tượng chứa các phần khai báo mới
              const additionalData = {
                maToChucDvt,
                soTien,
                soThang,
                maNhanVienThu,
                tenNhanVienThu,
                maCqBhxh,
                tenCqBhxh,
                key,
                tuNgay,
                denNgay,
              };

              // Thêm cả filteredItem và additionalData vào mảng dataKekhai
              dataKekhai.push({
                ...filteredItem,
                ...additionalData,
              });

              // tạo biên lai trước khi lưu hồ sơ kê khai
              // console.log(dataKekhai);
              // in biên lai
              const nowInVietnam = DateTime.now().setZone("Asia/Ho_Chi_Minh");
              // const formattedDate = nowInVietnam.toFormat(
              //   "dd-MM-yyyy HH:mm:ss"
              // );

              // thông tin biên lai
              const currentYear = new Date().getFullYear();
              // console.log(currentYear);

              // console.log(nextInvoice);

              // console.log(this.items[i].sotien);
              const tiendong = parseInt(
                this.items[i].sotien.replace(/,/g, ""),
                10
              );

              // lấy tên biên lai để lưu
              const formattedForFilename = formattedDate.replace(/[-: ]/g, "_");
              const urlNameInvoice = `${this.items[i].hosoIdentity}_${formattedForFilename}_${this.items[i].hoten}`;

              // console.log(urlNameInvoice);

              const dataPost = {
                hosoIdentity: this.items[i].hosoIdentity,
                maSoBhxh: this.items[i].masobhxh,
                hoTen: this.items[i].hoten,
                soCccd: this.items[i].cccd,
                ngaySinh: this.items[i].ngaysinh,
                gioiTinh: this.items[i].gioitinh,
                soDienThoai: this.items[i].dienthoai,
                nguoithutien: this.items[i].tennguoitao,
                loaiDt: this.items[i].tenloaihinh,
                soTien: tiendong,
                soThang: this.items[i].maphuongthucdong,
                tuNgay: this.items[i].tungay,
                denNgay: this.items[i].denngay,
                tuThang: this.items[i].tuthang,
                denThang: this.items[i].denthang,
                maDaiLy: this.items[i].madaily,
                tenDaiLy: this.items[i].tendaily,
                createdBy: this.user.username,
                sobienlai: "",
                ngaybienlai: formattedDate,
                maloaihinh: this.items[i].maloaihinh,
                tothon: this.items[i].tothon,
                tenquanhuyen: this.items[i].tenquanhuyen,
                tentinh: this.items[i].tentinh,
                currentYear: currentYear,
                urlNameInvoice: urlNameInvoice,
              };

              const ghibienlai = await this.$axios.post(
                `/api/kekhai/ghidulieubienlai`,
                dataPost
              );

              // lưu biên lai vào máy chủ
              await this.inBienLaiDientu(dataPost);
              // console.log("xongbienlai");
            }

            // console.log(dataKekhai);

            const result = await this.$axios.post(
              `/api/kekhai/add-kekhai-series`,
              dataKekhai
            );

            if (result.status === 200) {
              Swal.fire({
                title: "Kê khai thành công hồ sơ!",
                // text: "Đã gửi thông tin hồ sơ lên cổng BHXH VN!",
                icon: "success",
              });

              this.isLoading = false;
              this.isActive_xacnhan = false;
              this.items = [];
            }
          } catch (error) {
            // console.log(error);
            this.isLoading = false;
          }
        }
      }
    },

    async inBienLaiDientu(data) {
      const doc = new jsPDF({
        orientation: "l",
        format: "a5",
      });

      // Kích thước trang PDF
      const pageWidth = doc.internal.pageSize.getWidth();
      const pageHeight = doc.internal.pageSize.getHeight();

      // Kích thước ảnh bạn muốn (ví dụ: 100mm x 70mm)
      const imageWidth = 100; // Chiều rộng của ảnh
      const imageHeight = 70; // Chiều cao của ảnh

      // Tính tọa độ để ảnh nằm chính giữa trang
      const x = (pageWidth - imageWidth) / 2; // Căn giữa theo chiều ngang
      const y = (pageHeight - imageHeight) / 2; // Căn giữa theo chiều dọc

      // Thêm ảnh vào PDF
      doc.addImage(backgroundImage, "PNG", x, y, imageWidth, imageHeight);
      const img = new Image();
      img.src = backgroundImage; // hoặc base64 string

      // img.onload = () => {
      //   console.log("✅ Ảnh đã load xong");
      //   doc.addImage(img, "PNG", x, y, imageWidth, imageHeight);
      //   console.log("➡️ Đã add image");
      // };

      // img.onerror = (err) => {
      //   console.error("❌ Lỗi load ảnh:", err);
      // };

      // add the font to jsPDF
      doc.addFont("OpenSans-Bold-normal.ttf", "OpenSans-Bold", "bold");
      doc.setFont("OpenSans-Bold", "bold");
      doc.setFontSize(12);
      doc.setTextColor("#04368c");
      doc.text(`${company.bhxhName}`, 60, 10, {
        align: "center",
        fontWeight: "bold",
      });

      doc.setFontSize(12);
      doc.setTextColor("ff0000");
      doc.text(`${company.companyName}`, 60, 17, {
        align: "center",
        fontWeight: "bold",
      });

      // Đặt màu cho đường line (gạch chân)
      doc.setDrawColor(248, 215, 218);
      doc.setLineWidth(0.4); // Độ dày đường gạch

      const y_line = 19; // Vị trí theo chiều dọc

      // Di chuyển sang trái nhiều hơn và rút ngắn chiều dài
      const x1 = 40; // điểm bắt đầu (trái)
      const lineLength = 42; // chiều dài line
      const x2 = x1 + lineLength;
      doc.line(x1, y_line, x2, y_line);

      doc.addFont("OpenSans-Bold-normal.ttf", "OpenSans-Bold", "bold");
      doc.setFont("OpenSans-Bold", "bold");
      doc.setFontSize(10);
      doc.setTextColor("#04368c");
      doc.text(`Mẫu số: C45-BB `, 173, 11, {
        align: "center",
        fontWeight: "bold",
      });

      doc.addFont(
        "OpenSans_SemiCondensed-Italic-normal.ttf",
        "OpenSans_SemiCondensed-Italic-normal",
        "italic"
      );
      doc.setFont("OpenSans_SemiCondensed-Italic-normal", "italic");
      doc.setFontSize(9);
      doc.setTextColor("#04368c");
      doc.text(`(Ban hành kèm theo Thông tư số 107/2017/TT-BTC `, 175, 15, {
        align: "center",
        fontWeight: "bold",
      });
      doc.text(`ngày 10/10/2017 của Bộ Tài chính) `, 175, 19, {
        align: "center",
        fontWeight: "bold",
      });

      doc.addFont(
        "OpenSans-ExtraBold-normal.ttf",
        "OpenSans-ExtraBold-normal",
        "bold"
      );
      doc.setFont("OpenSans-ExtraBold-normal", "bold");
      doc.setFontSize(20);
      doc.setTextColor("#dc143c");
      doc.text(`BIÊN LAI THU TIỀN `, 105, 35, {
        align: "center",
        fontWeight: "bold",
      });

      doc.addFont(
        "OpenSans_SemiCondensed-Italic-normal.ttf",
        "OpenSans_SemiCondensed-Italic-normal",
        "italic"
      );
      doc.setFont("OpenSans_SemiCondensed-Italic-normal", "italic");
      doc.setFontSize(9);
      doc.setTextColor("#00008b");
      doc.text(
        `Do ${company.companyNameThuong}, tổ chức được Bảo hiểm xã hội uỷ quyền thu phát hành. `,
        105,
        41,
        {
          align: "center",
          fontWeight: "bold",
        }
      );

      const ngayBienLai = data.ngaybienlai.split(" ")[0];
      doc.setFontSize(9);
      doc.setTextColor("#00008b");
      doc.text(`Ngày: `, 155, 50, {
        fontWeight: "bold",
      });
      doc.text(`${ngayBienLai}`, 165, 50, {
        fontWeight: "bold",
      });

      const year = data.ngaybienlai.split("-")[2].split(" ")[0];

      doc.text(`Ký hiệu: `, 155, 55, {
        fontWeight: "bold",
      });
      doc.text(`${data.maloaihinh}-${data.maDaiLy}-${year}`, 165, 55, {
        fontWeight: "bold",
      });

      doc.text(`Số: `, 155, 60, {
        fontWeight: "bold",
      });
      doc.text(`${data.sobienlai}`, 165, 60, {
        fontWeight: "bold",
      });

      doc.addImage(qrcode, "PNG", 165, 25, 15, 15);
      //font-times-new-roman-normal
      const toadoXInfo = 10;
      const toadoYInfo = 60;
      doc.addFont(
        "Times New Roman Bold-normal.ttf",
        "Times New Roman Bold-normal",
        "bold"
      );
      doc.setFont("Times New Roman Bold-normal", "bold");
      doc.setFontSize(12);
      doc.setTextColor("#04368c");
      doc.text(`Họ và tên người nộp:`, toadoXInfo, toadoYInfo, {
        fontWeight: "bold",
      });
      doc.text(
        `${data.hoTen} - Mã số BHXH: ${data.maSoBhxh}`,
        toadoXInfo + 43,
        toadoYInfo,
        {
          fontWeight: "bold",
        }
      );

      const diachi = data.tenxaphuong + "; " + data.tentinh;
      // data.tothon + "; " +

      doc.text(`Địa chỉ: `, toadoXInfo, toadoYInfo + 8, {
        fontWeight: "bold",
      });
      doc.text(`${diachi}`, toadoXInfo + 16, toadoYInfo + 8, {
        fontWeight: "bold",
      });

      var noidungText = "";

      if (data.maloaihinh == "AR" || data.maloaihinh == "BI") {
        noidungText = `Tiền đóng BHYT, phương thức đóng ${data.soThang} tháng, từ ngày ${data.tuNgay} đến ngày ${data.denNgay}`;
      } else {
        if (data.maphuongan !== "DB") {
          noidungText = `Tiền đóng BHXH Tự nguyện, phương thức đóng ${data.soThang} tháng, từ ngày ${data.tuThang} đến ngày ${data.denThang}`;
        } else {
          noidungText = `BHXH Tự nguyện, ${data.tenphuongthucdong}, ${data.sothang} tháng, từ tháng ${data.tuThang}`;
        }
      }
      doc.text(`Nội dung: `, toadoXInfo, toadoYInfo + 16, {
        fontWeight: "bold",
      });
      doc.text(`${noidungText}`, toadoXInfo + 20, toadoYInfo + 16, {
        fontWeight: "bold",
      });

      const formattedMoney = Number(data.soTien).toLocaleString("vi-VN");
      // console.log(formattedMoney);

      doc.text(`Số tiền thu: `, toadoXInfo, toadoYInfo + 24, {
        fontWeight: "bold",
      });
      doc.text(`${formattedMoney}`, toadoXInfo + 24, toadoYInfo + 24, {
        fontWeight: "bold",
      });

      doc.text(`(Loại tiền): VNĐ `, toadoXInfo + 100, toadoYInfo + 24, {
        fontWeight: "bold",
      });

      let tienbangchuText = num2words(data.soTien);
      let tienHoa = this.capitalizeFirstLetter(tienbangchuText);
      tienHoa += " đồng./.";


      doc.text(`(Viết bằng chữ: ${tienHoa}) `, toadoXInfo, toadoYInfo + 32, {
        fontWeight: "bold",
      });

      doc.addFont(
        "OpenSans-ExtraBold-normal.ttf",
        "OpenSans-ExtraBold-normal",
        "bold"
      );
      doc.setFont("OpenSans-ExtraBold-normal", "bold");
      doc.setFontSize(13);
      doc.setTextColor("#04368c");
      doc.text(`NGƯỜI NỘP TIỀN`, toadoXInfo + 20, toadoYInfo + 43, {
        fontWeight: "bold",
      });

      doc.text(`NGƯỜI THU TIỀN`, toadoXInfo + 120, toadoYInfo + 43, {
        fontWeight: "bold",
      });

      doc.addFont(
        "OpenSans-ExtraBold-normal.ttf",
        "OpenSans-ExtraBold-normal",
        "bold"
      );
      doc.setFont("OpenSans-ExtraBold-normal", "bold");
      doc.setFontSize(11);
      doc.setTextColor("#04368c");
      doc.text(`${data.hoTen}`, toadoXInfo + 40, toadoYInfo + 75, {
        fontWeight: "bold",
        align: "center",
      });

      doc.setFontSize(8);
      doc.setTextColor("#dc3545");
      doc.text(
        `Đã được ký bởi: ${company.companyName}`,
        toadoXInfo + 106,
        toadoYInfo + 53,
        {
          fontWeight: "bold",
        }
      );
      doc.text(
        `Ngày ký: ${ngayBienLai}`,
        toadoXInfo + 124,
        toadoYInfo + 58,
        {
          fontWeight: "bold",
        }
      );

      doc.addFont(
        "OpenSans-ExtraBold-normal.ttf",
        "OpenSans-ExtraBold-normal",
        "bold"
      );
      doc.setFont("OpenSans-ExtraBold-normal", "bold");
      doc.setFontSize(11);
      doc.setTextColor("#04368c");

      // Tâm mong muốn theo trục X
      const centerX = toadoXInfo + 128;
      doc.text(`${this.user.name}`, centerX + 11, toadoYInfo + 75, {
        align: "center",
      });

      doc.addFont(
        "OpenSans_SemiCondensed-Italic-normal.ttf",
        "OpenSans_SemiCondensed-Italic-normal",
        "italic"
      );
      doc.setFont("OpenSans_SemiCondensed-Italic-normal", "italic");
      doc.setFontSize(8);
      doc.setTextColor("#04368c");
      doc.text(
        `Vui lòng tra cứu biên lai điện tử tại: `,
        toadoXInfo + 54,
        toadoYInfo + 82,
        {
          fontWeight: "bold",
        }
      );

      doc.setFontSize(8);
      doc.setTextColor("#dc143c");
      doc.text(
        `${company.urlBienlaidientu}`,
        toadoXInfo + 92,
        toadoYInfo + 82,
        {
          fontWeight: "bold",
        }
      );

      const tenbienlai = data.urlNameInvoice;

      const pdfBlob = doc.output("blob");

      const formData = new FormData();
      formData.append("pdf", pdfBlob, `${tenbienlai}.pdf`);

      // Gửi về backend
      await this.$axios.post("/api/kekhai/upload-bienlai", formData, {
        headers: {
          "Content-Type": "multipart/form-data",
        },
      });
    },

    async onFileChange(event) {
      const file = event.target.files[0];
      if (!file) {
        Swal.fire("Lỗi", "Chưa chọn file", "error");
        return;
      }

      const reader = new FileReader();
      reader.onload = (e) => {
        const binaryString = e.target.result;
        const workbook = XLSX.read(binaryString, { type: "binary" });

        // Giả sử file của bạn có dữ liệu ở sheet đầu tiên
        const sheet = workbook.Sheets[workbook.SheetNames[0]];
        const jsonData = XLSX.utils.sheet_to_json(sheet);
        
        // Kiểm tra và xử lý dữ liệu sau khi đọc từ file Excel
        if (jsonData.length > 0) {
          jsonData.forEach((row) => {
            this.addRowFromImportedData(row);
          });
        }
      };
      
      reader.readAsBinaryString(file);
    },

    // Phương thức này sẽ thêm dữ liệu vào danh sách `items` từ file Excel
    addRowFromImportedData(rowData) {
      try {
        this.items.push({
          ...rowData,
          // Thêm các trường dữ liệu mặc định nếu cần
          trangthai: 0,
          hinhthucnap: 1,
        });
      } catch (error) {
        console.error("Lỗi khi thêm dữ liệu từ file:", error);
      }

      console.log(this.items);
      
    },

    capitalizeFirstLetter(str) {
      if (!str) return "";
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    async importKekhai() {
      // thêm các thứ sau. 1. import XLSX from "xlsx"
      // 2. thêm isActive_import (vào data); 3. thêm fileName: "", và selectedFile: null, vào data
      // thêm @click="importKekhai" cho button trên cùng
      this.isActive_import = true;
    },

    cancelImport() {
      this.isActive_import = false;
      this.items = [];
    },
  },
};
</script>

<style scoped lang="css">
@import "@/assets/customCss/common.css";

@import "@/assets/customCss/footerTable.css";

/* Tùy chỉnh chiều cao của danh sách */
.vs__dropdown-menu {
  max-height: 50px; /* Giảm chiều cao của danh sách */
  overflow-y: auto; /* Thêm thanh cuộn */
}

/* Tùy chỉnh chiều cao item trong danh sách */
.vs__dropdown-option {
  line-height: 1.2; /* Giảm chiều cao của mỗi mục */
  padding: 4px 8px; /* Tùy chỉnh padding */
}

/* Tùy chỉnh hướng xổ lên */
.vs__dropdown-container {
  position: absolute !important;
  transform: translateY(-100%) !important;
}

  /* Mặc định cho thiết bị di động */
  .modal-card-predata {
    max-height: 80vh; /* Chiều cao tối đa là 80% màn hình */
    overflow-y: auto; /* Cho phép cuộn nếu nội dung quá dài */
  }

  /* Cho thiết bị máy tính */
  @media (min-width: 1024px) {
    .modal-card-predata {
      max-height: 90vh; /* Tăng chiều cao tối đa cho máy tính */
    }
  }

  

</style>
