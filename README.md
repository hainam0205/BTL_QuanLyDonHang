# BTL_QuanLyDonHang
Bài tập lớn Python quản lý đơn hàng trực tuyến
import json
class DonHang:
    def __init__(self, ma_dh, ten_kh, san_pham, tong_tien, trang_thai):
        self.ma_dh = ma_dh
        self.ten_kh = ten_kh
        self.san_pham = san_pham  
        self.tong_tien = tong_tien
        self.trang_thai = trang_thai

    def to_dict(self):
        return {
            "ma_dh": self.ma_dh,
            "ten_kh": self.ten_kh,
            "san_pham": self.san_pham,
            "tong_tien": self.tong_tien,
            "trang_thai": self.trang_thai
        }

    def from_dict(data):
        return DonHang(data["ma_dh"], data["ten_kh"], data["san_pham"], data["tong_tien"], data["trang_thai"])


class QuanLyDonHang:
    def __init__(self):
        self.ds_dh = []

    def khoi_tao(self):
        self.ds_dh = []
        print("Danh sách đơn hàng đã được khởi tạo.")

    def danh_sach_rong(self):
        return len(self.ds_dh) == 0

    def them_dau(self, dh):
        self.ds_dh.insert(0, dh)
        print("Đã thêm đơn hàng vào đầu danh sách.")

    def them_cuoi(self, dh):
        self.ds_dh.append(dh)
        print("Đã thêm đơn hàng vào cuối danh sách.")

    def sap_xep_tong_tien(self):
        self.ds_dh.sort(key=lambda x: x.tong_tien)
        print("Đã sắp xếp danh sách đơn hàng theo tổng tiền.")

    def hien_thi(self):
        if not self.ds_dh:
            print(" Danh sách đơn hàng rỗng.")
        else:
            print("\n DANH SÁCH ĐƠN HÀNG:")
            for dh in self.ds_dh:
                print(f"Mã ĐH: {dh.ma_dh} | KH: {dh.ten_kh} | Tổng tiền: {dh.tong_tien:.2f} VNĐ | Trạng thái: {dh.trang_thai}")

    def tong_doanh_thu(self):
        return sum(dh.tong_tien for dh in self.ds_dh)

    def dh_doanh_thu_max(self):
        return max(self.ds_dh, key=lambda x: x.tong_tien, default=None)

    def dh_doanh_thu_min(self):
        return min(self.ds_dh, key=lambda x: x.tong_tien, default=None)

    def tim_ma_dh(self, ma_dh):
        return next((dh for dh in self.ds_dh if dh.ma_dh == ma_dh), None)

    def tim_ten_kh(self, ten_kh):
        return [dh for dh in self.ds_dh if ten_kh.lower() in dh.ten_kh.lower()]

    def xoa_ma_dh(self, ma_dh):
        truoc = len(self.ds_dh)
        self.ds_dh = [dh for dh in self.ds_dh if dh.ma_dh != ma_dh]
        if len(self.ds_dh) < truoc:
            print(" Đã xóa đơn hàng.")
        else:
            print(" Không tìm thấy mã đơn hàng.")

    def dem_don_hang(self):
        return len(self.ds_dh)

    def ghi_file(self, ten_file):
        with open(ten_file, "w", encoding="utf-8") as f:
            json.dump([dh.to_dict() for dh in self.ds_dh], f, ensure_ascii=False, indent=2)
        print(f" Đã ghi danh sách đơn hàng vào file {ten_file}")

    def doc_file(self, ten_file):
        try:
            with open(ten_file, "r", encoding="utf-8") as f:
                data = json.load(f)
                self.ds_dh = [DonHang.from_dict(dh) for dh in data]
            print(f" Đã đọc danh sách đơn hàng từ file {ten_file}")
        except FileNotFoundError:
            print(" File không tồn tại.")

def nhap_don_hang():
    ma_dh = input(" Nhập mã đơn hàng: ")
    ten_kh = input(" Nhập tên khách hàng: ")
    san_pham = input(" Nhập sản phẩm (vd: sp1, sp2): ").split(', ')
    while True:
        try:
            tong_tien = float(input(" Nhập tổng tiền: "))
            break
        except ValueError:
            print(" Tổng tiền phải là số.")
    trang_thai = input(" Nhập trạng thái đơn hàng: ")
    return DonHang(ma_dh, ten_kh, san_pham, tong_tien, trang_thai)


# Menu
def menu():
    ql = QuanLyDonHang()
    while True:
        print("\n--------- MENU QUAN LY DON HANG ---------")
        print("1. Khoi tao danh sach don hang")
        print("2. Kiem tra danh sach co rong")
        print("3. Them don hang vao dau danh sach")
        print("4. Them don hang vao cuoi danh sach")
        print("5. Sap xep don hang theo tong tien")
        print("6. Hien danh sach don hang")
        print("7. Tinh tong doanh thu")
        print("8. Tim don hang co doanh thu cao nhat")
        print("9. Tim don hang co doanh thu thap nhat")
        print("10. Tim don hang theo ma")
        print("11. Tim don hang theo ten khach hang")
        print("12. Xoa don hang theo ma")
        print("13. Dem so luong don hang")
        print("14. Xuat danh sach ra file")
        print("15. Doc danh sach tu file")
        print("0. Thoat")
        chon = input("👉 Nhap lua chon: ")

        if chon == '1':
            ql.khoi_tao()
        elif chon == '2':
            print(" Danh sách rỗng." if ql.danh_sach_rong() else "📦 Danh sách có dữ liệu.")
        elif chon == '3':
            dh = nhap_don_hang()
            ql.them_dau(dh)
        elif chon == '4':
            dh = nhap_don_hang()
            ql.them_cuoi(dh)
        elif chon == '5':
            ql.sap_xep_tong_tien()
        elif chon == '6':
            ql.hien_thi()
        elif chon == '7':
            print(f" Tổng doanh thu: {ql.tong_doanh_thu():,.2f} VNĐ")
        elif chon == '8':
            dh = ql.dh_doanh_thu_max()
            if dh:
                print(f" Đơn hàng có doanh thu cao nhất: Mã ĐH {dh.ma_dh} ({dh.tong_tien:.2f} VNĐ)")
            else:
                print(" Không có dữ liệu.")
        elif chon == '9':
            dh = ql.dh_doanh_thu_min()
            if dh:
                print(f" Đơn hàng có doanh thu thấp nhất: Mã ĐH {dh.ma_dh} ({dh.tong_tien:.2f} VNĐ)")
            else:
                print(" Không có dữ liệu.")
        elif chon == '10':
            ma_dh = input(" Nhập mã đơn hàng cần tìm: ")
            dh = ql.tim_ma_dh(ma_dh)
            if dh:
                print(f"🔍 Mã ĐH: {dh.ma_dh} | KH: {dh.ten_kh} | Tổng tiền: {dh.tong_tien:.2f} VNĐ | Trạng thái: {dh.trang_thai}")
            else:
                print(" Không tìm thấy.")
        elif chon == '11':
            ten_kh = input(" Nhập tên khách hàng cần tìm: ")
            ds = ql.tim_ten_kh(ten_kh)
            if ds:
                for dh in ds:
                    print(f"- Mã ĐH: {dh.ma_dh} | KH: {dh.ten_kh} | Tổng tiền: {dh.tong_tien:.2f} VNĐ | Trạng thái: {dh.trang_thai}")
            else:
                print(" Không có đơn hàng phù hợp.")
        elif chon == '12':
            ma_dh = input(" Nhập mã đơn hàng cần xóa: ")
            ql.xoa_ma_dh(ma_dh)
        elif chon == '13':
            print(f" Tổng số đơn hàng: {ql.dem_don_hang()}")
        elif chon == '14':
            ql.ghi_file("donhang.json")
        elif chon == '15':
            ql.doc_file("donhang.json")
        elif chon == '0':
            print(" Thoát chương trình.")
            break
        else:
            print(" Lựa chọn không hợp lệ.")

if __name__ == "__main__":
    menu()
