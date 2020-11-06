# LanZouLogin
蓝奏云登录器  
自动过阿里滑块  
支持上传下载  

![image](https://github.com/laomms/LanZouLogin/blob/main/111.jpg)   


selenium过滑块代码:

```vb.net
 Try
                Using driver As IWebDriver = New ChromeDriver(DriverService, options)    
                    driver.Navigate().GoToUrl(uri)
                    Thread.Sleep(1000)
                    Dim username As IWebElement = driver.FindElement(By.XPath("//input[contains(@id, 'username')]"))
                    username.SendKeys(szUsername)
                    Dim password As IWebElement = driver.FindElement(By.XPath("//input[contains(@class, 'input pwd')]"))
                    password.SendKeys(szPassword)
                    Dim slide = driver.FindElement(By.CssSelector("#nc_1_n1z"))
                    Dim verifyContainer = driver.FindElement(By.CssSelector(".nc-lang-cnt"))
                    Dim width = verifyContainer.Size.Width
                    Dim action = New Actions(driver)
                    action.ClickAndHold(slide).Perform()
                    Dim random As New Random()
                    Dim offset As Integer = 0
                    Const minOffset As Integer = 10
                    Const maxOffset As Integer = 30
                    Do While width > offset
                        offset += random.Next(minOffset, maxOffset)
                        action.MoveByOffset(offset, 0).Perform()
                        Dim code = driver.FindElement(By.CssSelector(".nc-lang-cnt")).Text
                        If code.Contains("验证通过") Then
                            Exit Do
                        End If
                        System.Threading.Thread.Sleep(offset * minOffset)
                    Loop
                    driver.FindElement(By.Id("s3")).Click()
                    Thread.Sleep(100)
                    Dim str = driver.PageSource
                    If str.Contains("确认退出系统") Or str.Contains("登录成功") Then
                        Dim cc As New CookieCollection()
                        For Each cook As OpenQA.Selenium.Cookie In driver.Manage().Cookies.AllCookies
                            Dim cookie As New System.Net.Cookie()
                            If cook.Name.Contains("ylogin") Then ylogin = cook.Value                  
                            If cook.Name.Contains("phpdisk_info") Then phpdiskinfo = cook.Value                                 
                            cookie.Name = cook.Name
                            cookie.Value = cook.Value
                            cookie.Domain = cook.Domain
                            cc.Add(cookie)
                        Next
                        mycookiecontainer.Add(cc)
                        DriverService.Dispose()
                        Return True
                    End If
                End Using
            Catch ex As Exception
                DriverService.Dispose()
                Return False
            End Try
```   
