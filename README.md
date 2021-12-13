# gitflow

- branch di upstream -> main (deploy to production), dev(deploy to uat)
- level -> maintainer, developer

- jenis branch di origin developer -> feature/bugfix/hotfix
- scope jenis branch :
  1. feature = fitur baru
  2. bugfix = bugfix dari fitur yang dikembangkan (masih di uat)
  2. hotfix = bugfix dari fitur/issue yang sudah direlease/production 

- penamaan branch -> [jenis_branch]/#[no_feature_or_issue]
- penamaan commit -> [message]

- versioning -> Vx.y.z (semantic) :
  1. x = major (penambahan besar berupa fitur yang menyebabkan sebagian besar fitur lama tidak dipakai lagi)
  2. y = minor (penambahan fitur tanpa mempengaruhi fitur lama)
  3. z = patch (bugfix dan hotfix)

===========================================================================================================

# practice

- Maintainer create repo
- Maintainer create branch dev dari main
  `git checkout -b dev` dan `git push origin dev`
- Maintainer install `AutoChangelog`
  `npm install -g auto-changelog`

- Developer melakukan fork repo upstream
- Developer melakukan cloning repo origin
  `git clone <URL HTTPS REPO ORIGIN>`
- Developer melakukan mapping repo origin dengan repo upstream
  `git remote add upstream <URL HTTPS REPO UPSTREAM>`

--------------------------------------------------------------------------------------------------
- CASE 1 -> developing feature (belum sampai prod)
  * Developer
    - Pastikan di branch `dev`
      `git checkout dev`
    - Pull terbaru dari upstream dev
      `git pull upstream dev`
    - Bikin branch baru terkait pengembangan fitur (cth: fitur chat)
      `git checkout -b feature/chat`
    - Coding/development
    - Pull terbaru dari upstream dev
      `git pull upstream dev`
    - Fixing konflik jika ada
    - Commit fitur
      `git add .` dan `git commit -m "add fitur chat #IDE-NU-001"`
    - Merge fitur ke branch dev
      `git checkout dev`
      `git merge feature/chat`
      `git push origin dev`
    - Create PR dari `origin dev` ke `upstream dev`
    - Jika ada comment, maka ulangi dari proses `Coding` sampai `Merge fitur ke branch dev`
    - Hapus branch `feature/chat` jika sudah selesai dicek oleh maintainer (tidak ada comment lagi)
      `git branch -d feature/chat`

  * Maintainer
    - Review code dan merge PR ke `upstream dev` jika sudah sesuai
    - Review code dan comment PR jika ada temuan
    - Deploy ke server uat

--------------------------------------------------------------------------------------------------
- CASE 2 -> bugfix development (ada bugfix temuan dari BA/QA saat testing di uat)
  * Developer
    - Pastikan di branch `dev`
      `git checkout dev`
    - Pull terbaru dari upstream dev
      `git pull upstream dev`
    - Bikin branch baru terkait bugfix fitur
      `git checkout -b bugfix/timestamp_chat_issue`
    - Coding/development
    - Pull terbaru dari upstream dev
      `git pull upstream dev`
    - Fixing konflik jika ada
    - Commit fitur
      `git add .` dan `git commit -m "fix timestamp tidak sesuai di chat #IDE-NU-002"`
    - Merge fitur ke branch dev
      `git checkout dev`
      `git merge bugfix/timestamp_chat_issue`
      `git push origin dev`
    - Create PR dari `origin dev` ke `upstream dev`
    - Jika ada comment, maka ulangi dari proses `Coding` sampai `Merge fitur ke branch dev`
    - Hapus branch `feature/chat` jika sudah selesai dicek oleh maintainer
      `git branch -d bugfix/timestamp_chat_issue`

  * Maintainer
    - Review code dan merge PR ke `upstream dev` jika sudah sesuai
    - Review code dan comment PR jika ada temuan
    - Deploy ke server uat
    - Lulus tes dari BA/QA
    - Clone repo upstream (jika belum punya reponya)
      `git clone <URL HTTPS REPO UPSTREAM>`
    - Pindah ke direktori projek
      `cd <Directory>`
    - Checkout ke branch `dev`
      `git checkout dev`
    - Bikin branch `release`
      `git checkout -b release/v1.0.0`
    - Branch `release` hanya boleh diupdate jika ada bugfix, dokumentasi tambahan atau apapun yang terkait release (tidak boleh ada fitur baru)
    - Berikan tag version di branch `release`
      `git tag v1.0.0`
    - Bikin changelog file
      `auto-changelog`
    - Commit fitur
      `git add .` dan `git commit -m "release fitur chat #IDE-NU-001"`
    - Merge fitur ke `main`
      `git checkout main`
      `git merge v1.0.0`
      `git push origin v1.0.0` (push tag)
      `git push origin master`
    - Hapus branch release di local
      `git branch -d release/v1.0.0`

--------------------------------------------------------------------------------------------------
- CASE 3 -> Ada update terkait `release` (bugfix, dokumentasi tambahan atau apapun yang terkait release, kondisi belum release ke main/prod)
  * Developer
    - Pull semua branch dari upstream (untuk mendapatkan branch release)
      `git pull upstream`
    - Checkout branch `release`
      `git checkout release/v1.0.0`
    - Bikin branch baru terkait hotfix issue
      `git checkout -b bugfix/swagger_update_api_send_chat`
    - Coding/development
    - Pull terbaru dari upstream main
      `git pull upstream release/v1.0.0`
    - Fixing konflik jika ada
    - Commit fitur
      `git add .` dan `git commit -m "add swagger chat #IDE-NU-004"`
      `git push origin release/v1.0.0`
    - Create PR dari `origin release/v1.0.0` ke `upstream release/v1.0.0`
    - Jika ada comment, maka ulangi dari proses `Coding` sampai `Commit fitur`
    - Hapus branch `release/v1.0.0` jika sudah selesai dicek oleh maintainer
      `git branch -d release/v1.0.0`

  * Maintainer
    - Push branch `release` ke `upstream` agar developer bisa narik di forked mereka
      `git checkout release/v1.0.0` dan `git push origin release/v1.0.0`
    - Menunggu commit dan push dari developer terkait update release
    - Review code dan merge PR ke `upstream release/v1.0.0` jika sudah sesuai
    - Review code dan comment PR jika ada temuan
    - Checkout branch `dev`
      `git checkout dev`
    - Merge branch `release` ke `dev` (lokal)
      `git merge release/v1.0.0`
    - Deploy ke server uat
    - Lulus tes dari BA/QA
    - Checkout ke branch `release`
      `git checkout release/v1.0.0`
    - Berikan tag version di branch `release`
      `git tag v1.0.0`
    - Bikin changelog file
      `auto-changelog`
    - Commit fitur
      `git add .` dan `git commit -m "release fitur chat #IDE-NU-001"`
    - Merge fitur ke `main`
      `git checkout main`
      `git merge v1.0.0`
      `git push origin v1.0.0` (push tag)
      `git push origin master`
    - Hapus branch `release` di `local`
      `git branch -d release/v1.0.0`
    - Hapus branch `release` di `remote` (gitcommand local atau delete manually via remote repo on web)

--------------------------------------------------------------------------------------------------
- CASE 4 -> hotfix (ada issue setelah release)
  * Developer
    - Checkout branch `main`
      `git checkout main`
    - Pull semua branch dari upstream (untuk mendapatkan branch hotfix)
      `git pull upstream`
    - Pull terbaru dari `upstream main`
      `git pull upstream main`
    - Bikin branch baru terkait hotfix issue
      `git checkout -b hotfix/background_chat_issue`
    - Coding/development
    - Pull terbaru dari upstream main
      `git pull upstream main`
    - Fixing konflik jika ada
    - Commit fitur
      `git add .` dan `git commit -m "fix background chat #IDE-NU-003"`
    - Merge fitur ke branch `hotfix`
      `git checkout hotfix`
      `git merge hotfix/background_chat_issue`
      `git push origin hotfix`
    - Create PR dari `origin hotfix` ke `upstream hotfix`
    - Jika ada comment, maka ulangi dari proses `Coding` sampai `Merge fitur ke branch hotfix`
    - Hapus branch `hotfix/background_chat_issue` jika sudah selesai dicek oleh maintainer
      `git branch -d hotfix/background_chat_issue`

  * Maintainer
    - Bikin branch baru `hotfix` dari main
      `git checkout main` dan `git checkout -b hotfix`
    - Push branch `hotfix` ke `upstream`
      `git push origin hotfix`
    - Menunggu PR dari developer terkait hotfix
    - Review code dan merge PR ke `upstream hotfix` jika sudah sesuai
    - Review code dan comment PR jika ada temuan
    - Checkout branch `dev`
      `git checkout dev`
    - Merge branch `hotfix` ke `dev` (lokal)
      `git merge hotfix`
    - Deploy ke server uat
    - Lulus tes dari BA/QA
    - Checkout ke branch `hotfix`
      `git checkout hotfix`
    - Bikin branch `release`
      `git checkout -b release/v1.0.1`
    - Berikan tag version di branch `release`
      `git tag v1.0.1`
    - Bikin changelog file
      `auto-changelog`
    - Commit fitur
      `git add .` dan `git commit -m "hotfix issue background chat #IDE-NU-003"`
    - Merge fitur ke `main`
      `git checkout main`
      `git merge v1.0.1`
      `git push origin v1.0.1` (push tag)
      `git push origin master`
    - Hapus branch `release` dan `hotfix` di `local`
      `git branch -d release/v1.0.1` dan `git branch -d hotfix`
    - Hapus branch `hotfix` di `remote` (gitcommand local atau delete manually via remote repo on web)