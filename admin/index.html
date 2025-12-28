---
layout: none
---
<!DOCTYPE html>
<html lang="en">

{% include head.html %}

<!-- hack iOS CSS :active style -->
<body ontouchstart="">

    {% include nav.html %}
    {% include search.html %}

    <!-- ########## 核心功能：拉黑检测 + 主站访问统计（适配私有Gist） ########## -->
    <script>
      // 1. 配置Gist信息
      const GITHUB_CONFIG = {
        gistId: "778c919fa51dbc8b932261916da783e7",
        gistFile: "blog_admin.json"
      };
      const BLOG_DOMAIN = "https://jin-cha-droid.github.io/";

      // 2. 读取本地PAT
      function getToken() {
        const token = localStorage.getItem('github_gist_token');
        if (!token) {
          console.log('未检测到PAT，统计功能受限（仅管理员可统计）');
          return null;
        }
        return token;
      }

      // 3. 生成唯一访客ID（和后台一致）
      function getVisitorId() {
        let id = localStorage.getItem('blog_visitor_id');
        if (!id) {
          id = Date.now() + '_' + Math.random().toString(36).substr(2, 10) + '_' + navigator.userAgent.substring(0, 15);
          localStorage.setItem('blog_visitor_id', id);
        }
        return id;
      }

      // 4. 读取Gist数据（通用函数）
      async function loadGistData() {
        const token = getToken();
        if (!token) return { bannedIds: [], notice: "", stats: { visitors: [], pageViews: [], allVisits: [] } };

        try {
          const res = await fetch(`https://api.github.com/gists/${GITHUB_CONFIG.gistId}`, {
            headers: {
              'Authorization': `token ${token}`,
              'Accept': 'application/vnd.github.v3+json'
            }
          });
          if (res.ok) {
            const gist = await res.json();
            const content = gist.files[GITHUB_CONFIG.gistFile]?.content || '{"bannedIds":[],"notice":"","stats":{"visitors":[],"pageViews":[],"allVisits":[]}}';
            return JSON.parse(content);
          }
          return { bannedIds: [], notice: "", stats: { visitors: [], pageViews: [], allVisits: [] } };
        } catch (err) {
          console.log('读取Gist数据失败：', err);
          return { bannedIds: [], notice: "", stats: { visitors: [], pageViews: [], allVisits: [] } };
        }
      }

      // 5. 主站访问统计（每一次页面加载都触发）
      async function reportBlogVisit() {
        const token = getToken();
        if (!token) return; // 仅管理员能统计（普通访客无PAT，不记录）

        const visitorId = getVisitorId();
        const now = new Date().toISOString();
        const currentPage = window.location.href;

        // 读取现有统计数据
        const gistData = await loadGistData();
        let { visitors, pageViews, allVisits } = gistData.stats;

        // 访客去重统计
        const visitorIndex = visitors.findIndex(v => v.id === visitorId);
        if (visitorIndex > -1) {
          visitors[visitorIndex].lastVisit = now;
          visitors[visitorIndex].visitCount = (visitors[visitorIndex].visitCount || 1) + 1;
        } else {
          visitors.push({
            id: visitorId,
            firstVisit: now,
            lastVisit: now,
            visitCount: 1
          });
        }

        // 页面访问统计
        const pageKey = currentPage.replace(BLOG_DOMAIN, '/');
        const pageIndex = pageViews.findIndex(p => p.pageKey === pageKey);
        if (pageIndex > -1) {
          pageViews[pageIndex].lastVisit = now;
          pageViews[pageIndex].visitCount = (pageViews[pageIndex].visitCount || 1) + 1;
        } else {
          pageViews.push({
            pageKey: pageKey,
            fullUrl: currentPage,
            visitCount: 1,
            firstVisit: now,
            lastVisit: now
          });
        }

        // 全量访问记录（每一次都存）
        allVisits.push({
          visitorId: visitorId,
          page: currentPage,
          time: now,
          userAgent: navigator.userAgent
        });

        // 保存统计数据
        gistData.stats.visitors = visitors;
        gistData.stats.pageViews = pageViews;
        gistData.stats.allVisits = allVisits;

        try {
          await fetch(`https://api.github.com/gists/${GITHUB_CONFIG.gistId}`, {
            method: 'PATCH',
            headers: {
              'Authorization': `token ${token}`,
              'Content-Type': 'application/json',
              'Accept': 'application/vnd.github.v3+json'
            },
            body: JSON.stringify({
              files: { [GITHUB_CONFIG.gistFile]: { content: JSON.stringify(gistData, null, 2) } }
            })
          });
        } catch (err) {
          console.log('主站统计保存失败：', err);
        }
      }

      // 6. 拉黑检测逻辑
      async function checkBanStatus() {
        const visitorId = getVisitorId();
        const gistData = await loadGistData();
        const bannedList = gistData.bannedIds || [];
        
        if (bannedList.includes(visitorId)) {
          document.body.innerHTML = `
            <div style="width: 100vw; height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; background: #f8f9fa; margin: 0; padding: 0;">
              <h1 style="color: #dc3545; font-size: 32px; margin-bottom: 20px;">🚫 You are banned from this site</h1>
              <p style="color: #6c757d; font-size: 16px;">Reason: Violated site access rules, contact admin for unban</p>
            </div>
          `;
          return false;
        }
        return true;
      }

      // 7. 读取并展示公告
      async function loadBlogNotice() {
        const token = getToken();
        if (!token) return;

        try {
          const gistData = await loadGistData();
          const noticeText = gistData.notice || "";
          
          if (noticeText.trim() !== "") {
            document.getElementById('notice-text').innerText = noticeText;
            document.getElementById('blog-notice-container').style.display = "flex";
          }
        } catch (err) {
          console.log('读取公告失败：', err);
        }
      }

      // 页面加载时执行：先检测拉黑 → 再统计访问 → 最后加载公告
      window.onload = async () => {
        const isNotBanned = await checkBanStatus();
        if (isNotBanned) {
          await reportBlogVisit(); // 主站访问统计
          await loadBlogNotice(); // 加载公告
        }
      };
    </script>

    <!-- 可关闭的红色警告样式公告条 -->
    <div id="blog-notice-container" style="display: none; background: #f8d7da; color: #721c24; padding: 12px 20px; border: 1px solid #f5c6cb; border-radius: 4px; margin: 10px 0; display: flex; justify-content: space-between; align-items: center;">
      <span id="notice-text"></span>
      <button onclick="this.parentElement.style.display='none'" style="background: transparent; border: none; color: #721c24; cursor: pointer; font-size: 16px; padding: 0 8px; line-height: 1;">&times;</button>
    </div>

    {{ content }}

    {% include footer.html %}

<!-- Image to hack wechat -->
<img src="/img/icon_wechat.png" width="0" height="0" />
<!-- Migrate from head to bottom, no longer block render and still work -->

</body>

</html>
