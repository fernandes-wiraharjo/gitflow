# gitflow

- branch di upstream -> main (deploy to production), dev(deploy to uat)
- level -> maintainer, developer

- jenis branch di origin developer -> feature/bugfix

- penamaan branch -> [jenis_branch]/#[no_feature_or_issue]_[fitur/isunya] -> cth: bugfix/#3_datetime_chat_not_updated
- penamaan commit -> [message] #[no_feature_or_issue] -> cth: fixing datetime chat missing logic #3
- title PR -> [branch_tujuan_PR] #[no_feature_or_issue]_[fitur/isunya] -> cth: [dev] #3_datetime_chat_not_updated

- versioning -> Vx.y.z (semantic) :
  1. x = major (penambahan besar berupa fitur yang menyebabkan sebagian besar fitur lama tidak dipakai lagi)
  2. y = minor (penambahan fitur tanpa mempengaruhi fitur lama)
  3. z = patch (bugfix)

===========================================================================================================

# practice

- Maintainer create repo
- Maintainer create branch dev dari main
  `git checkout -b dev` dan `git push origin dev`

- Developer melakukan fork repo upstream
- Developer melakukan cloning repo origin
  `git clone <URL HTTPS REPO ORIGIN>`
- Developer melakukan mapping repo origin dengan repo upstream
  `git remote add upstream <URL HTTPS REPO UPSTREAM>`
  
- Developer melakukan cek remote yang telah ditambahkan
  `git remote -v`
  ```
    origin  https://github.com/aldyHelix/gitflow.git (fetch)
    origin  https://github.com/aldyHelix/gitflow.git (push)
    upstream        https://github.com/fernandes-wiraharjo/gitflow.git (fetch)
    upstream        https://github.com/fernandes-wiraharjo/gitflow.git (push)
  ```

--------------------------------------------------------------------------------------------------
- CASE 1 -> develop feature
  * Developer
    - Pastikan di branch `dev`
      `git checkout dev`
    - Pull terbaru dari upstream dev
      `git pull upstream dev`
    - Bikin branch baru terkait pengembangan fitur (cth: fitur chat)
      `git checkout -b feature/#1_chat`
    - Coding/development
    - Pull terbaru dari upstream dev
      `git pull upstream dev`
    - Fixing konflik jika ada
    - Commit fitur
      `git add .` dan `git commit -m "add fitur chat #1"`
      `git push origin feature/#1_chat`
    - Create PR dari `origin feature/#1_chat` ke `upstream dev`
    - Create PR dari `origin feature/#1_chat` ke `upstream main` jika di uat sudah aman
    - Hapus branch `feature/chat`
      `git branch -d feature/chat`

  * Maintainer
    - Review code dan merge PR ke `upstream dev` jika sudah sesuai
    - Review code dan comment PR jika ada temuan
    - Deploy ke server uat
    - Review code dan merge PR ke `upstream main` jika sudah sesuai (jika di uat sudah aman)
    - Review code dan comment PR jika ada temuan
    - Deploy ke server main
    - Bikin tag dan release di repo remote

--------------------------------------------------------------------------------------------------
- CASE 2 -> bugfix (ada issue di production)
  * Developer
    - Checkout branch `main`
      `git checkout main`
    - Pull terbaru dari `upstream main`
      `git pull upstream main`
    - Bikin branch baru terkait issue
      `git checkout -b bugfix/#2_background_chat_issue`
    - Coding/development
    - Pull terbaru dari upstream main
      `git pull upstream main`
    - Fixing konflik jika ada
    - Commit fitur
      `git add .` dan `git commit -m "fix background chat #2"`
      `git push origin bugfix/#2_background_chat_issue`
    - Create PR dari `origin bugfix/#2_background_chat_issue` ke `upstream dev`
    - Create PR dari `origin bugfix/#2_background_chat_issue` ke `upstream main`
    - Hapus branch `bugfix/#2_background_chat_issue`
      `git branch -d bugfix/#2_background_chat_issue`

  * Maintainer
    - Review code dan merge PR ke `upstream dev` jika sudah sesuai
    - Review code dan comment PR jika ada temuan
    - Deploy ke server uat
    - Review code dan merge PR ke `upstream main` jika sudah sesuai (jika di uat sudah aman)
    - Review code dan comment PR jika ada temuan
    - Deploy ke server main
    - Bikin tag dan release di repo remote
