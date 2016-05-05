## Get another view's progress

Class to handling networking requests

    // RDPUploadManager.h
    @protocol RDPUploadManagerDeleagte : NSObject
    - (void)RDPUploadManager:(RDPUploaManager *)manager didPostWithProgress:(NSFloat)progress;
    @end
    
    @interface RDPUploadManager
    
    @property (nonatomic, weak)id<RDPUploadManagerDelegate> delegate;
    
    // Post data 
    - (void)postData:(NSData);
    
    @end
    

    // Implementation
    @implementation RDPUploadManager
    
    - (id)init {
        self = [super init];
    } 
    
    - (void)postData:(NSData) {
    
        [afnetworkingManager post:data
                             progress:^(NSCFloat progress){
                                 if ([self.delegate responsesToSelector:@selector(didPostwithprogress:)]){
                                     [self.delegate didPostWithProgress:progress);
                                 }
                             }
                             success:^(){}
                             failure:^(){}
    }
    @end
    


Post Class

    @interface PostVC
    
    @property (nonatomic, strong)MainVC *mainViewController;
   
    @end
    
    @implementation PostVC
    [RDPUploadManager instance].delegate = mainViewController;
    [RDPUploadManager post:data];
    [self popFromViewController]
    @end