# Bài 4: Mô phỏng quy trình Hotfix & Gitflow thực tế

## 1. Mục tiêu

Thực hành quy trình Gitflow khi hệ thống production trên nhánh `main` phát sinh lỗi nghiêm trọng và cần sửa lỗi khẩn cấp.

Đồng thời đồng bộ bản sửa lỗi từ nhánh Hotfix về `develop` để tránh lỗi xuất hiện lại trong phiên bản tương lai.

## 2. Quy trình thực hiện

Ban đầu:

* Nhánh `main` chứa phiên bản ổn định `v1.0.0`.
* Nhánh `develop` đang phát triển tính năng mới.
* Khi phát hiện lỗi nghiêm trọng trên production, tạo nhánh `hotfix/v1.0.1` trực tiếp từ `main`.

Sau đó:

1. Sửa lỗi trên nhánh `hotfix/v1.0.1`.
2. Commit bản sửa lỗi.
3. Merge `hotfix/v1.0.1` vào `main`.
4. Tạo tag `v1.0.1`.
5. Merge `hotfix/v1.0.1` ngược lại vào `develop`.
6. Xóa nhánh Hotfix sau khi hoàn thành.

## 3. Sơ đồ phân nhánh

```text
                    hotfix/v1.0.1
                         |
                         | fix: patch user data exposure
                         v
v1.0.0 ──────── main ────●────── v1.0.1
    \                    /
     \                  /
      └── develop ─────●
          feature      merge hotfix
```

## 4. Lịch sử Git thực tế

```text
* 81663bd (HEAD -> develop) merge: sync hotfix v1.0.1 into develop
|\
| * 647ff1d (tag: v1.0.1, main) fix: patch user data exposure
* | 7f858c9 feat: add new feature
|/
* 3e49ec3 (tag: v1.0.0) chore: initial stable v1.0.0
```

Lịch sử trên thể hiện bản sửa lỗi được đưa vào `main` và sau đó merge ngược về `develop`.

## 5. Các lệnh chính

```bash
git checkout main

git checkout -b hotfix/v1.0.1

git add .
git commit -m "fix: patch user data exposure"

git checkout main
git merge hotfix/v1.0.1

git tag -a v1.0.1 -m "Release Hotfix 1.0.1"

git checkout develop
git merge hotfix/v1.0.1

git branch -d hotfix/v1.0.1
```

## 6. Kiểm tra kết quả

Kiểm tra các nhánh:

```bash
git branch -a
```

Kiểm tra tag:

```bash
git tag
```

Kết quả:

```text
v1.0.0
v1.0.1
```

Kiểm tra lịch sử merge:

```bash
git log --graph --oneline --all
```

## 7. Kết quả

Đã hoàn thành quy trình Hotfix theo Gitflow:

* `main` chứa bản sửa lỗi bảo mật.
* Tag `v1.0.1` được tạo cho phiên bản release.
* `develop` đã nhận bản sửa lỗi từ Hotfix.
* Nhánh `hotfix/v1.0.1` được xóa sau khi hoàn thành.
* Lịch sử Git thể hiện rõ quá trình tạo Hotfix, merge vào `main` và đồng bộ về `develop`.
