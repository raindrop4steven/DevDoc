## HUD message and hidden

          // Everything is successful
          MBProgressHUD *hud = [MBProgressHUD showHUDAddedTo:self.view animated:YES];

          // Configure for text only and offset down
          hud.mode = MBProgressHUDModeText;
          hud.labelText = @"Some message...";
          hud.margin = 10.f;
          hud.yOffset = 150.f;
          hud.removeFromSuperViewOnHide = YES;

          [hud hide:YES afterDelay:3];
          [self reset];