图像处理GUI

```matlab
classdef Image_processing_GUI_App < matlab.apps.AppBase

    % Properties that correspond to app components
    properties (Access = public)
        figure1         matlab.ui.Figure
        uibuttongroup8  matlab.ui.container.ButtonGroup
        p5              matlab.ui.control.Button
        p4              matlab.ui.control.Button
        p3              matlab.ui.control.Button
        p2              matlab.ui.control.Button
        uibuttongroup7  matlab.ui.container.ButtonGroup
        p1              matlab.ui.control.Button
        m7              matlab.ui.control.Button
        invc            matlab.ui.control.Button
        uibuttongroup3  matlab.ui.container.ButtonGroup
        text8           matlab.ui.control.Label
        slider4         matlab.ui.control.Slider
        text7           matlab.ui.control.Label
        text6           matlab.ui.control.Label
        text5           matlab.ui.control.Label
        slider3         matlab.ui.control.Slider
        slider2         matlab.ui.control.Slider
        slider1         matlab.ui.control.Slider
        text9           matlab.ui.control.Label
        slider5         matlab.ui.control.Slider
        uibuttongroup6  matlab.ui.container.ButtonGroup
        strel           matlab.ui.control.DropDown
        strelv          matlab.ui.control.EditField
        ero             matlab.ui.control.Button
        dia             matlab.ui.control.Button
        strels          matlab.ui.control.Button
        uibuttongroup5  matlab.ui.container.ButtonGroup
        m5              matlab.ui.control.Button
        m4              matlab.ui.control.Button
        m3              matlab.ui.control.Button
        m2              matlab.ui.control.Button
        m1              matlab.ui.control.Button
        uibuttongroup4  matlab.ui.container.ButtonGroup
        m6              matlab.ui.control.Button
        m9              matlab.ui.control.Button
        m8              matlab.ui.control.Button
        uibuttongroup2  matlab.ui.container.ButtonGroup
        f3              matlab.ui.control.Button
        f1              matlab.ui.control.Button
        f2              matlab.ui.control.Button
        uibuttongroup1  matlab.ui.container.ButtonGroup
        n2              matlab.ui.control.Button
        n4              matlab.ui.control.Button
        n3              matlab.ui.control.Button
        n1              matlab.ui.control.Button
        text4           matlab.ui.control.Label
        text3           matlab.ui.control.Label
        text2           matlab.ui.control.Label
        reset           matlab.ui.control.Button
        save            matlab.ui.control.Button
        exit            matlab.ui.control.Button
        load            matlab.ui.control.Button
        g5              matlab.ui.control.UIAxes
        g4              matlab.ui.control.UIAxes
        g3              matlab.ui.control.UIAxes
        g2              matlab.ui.control.UIAxes
        g1              matlab.ui.control.UIAxes
    end

    
    methods (Access = private)
        function updateg4(app, handles)
            r=handles.img(:,:,1);
            g=handles.img(:,:,2);
            b=handles.img(:,:,3);
            x=size(r);
            x=(1:x(1,2));
            r=r(1,:);
            g=g(1,:);
            b=b(1,:);
            axes(handles.g4);
            cla;
            plot(x,r,'r');
            hold on
            plot(x,g,'g');
            plot(x,b,'b');
            hold off;
            
            ImageData1 = reshape(handles.img(:,:,1), [size(handles.img, 1) * size(handles.img, 2) 1]);
            ImageData2 = reshape(handles.img(:,:,2), [size(handles.img, 1) * size(handles.img, 2) 1]);
            ImageData3 = reshape(handles.img(:,:,3), [size(handles.img, 1) * size(handles.img, 2) 1]);
            [H1, X1] = hist(ImageData1, 1:5:256);
            [H2, X2] = hist(ImageData2, 1:5:256);
            [H3, X3] = hist(ImageData3, 1:5:256);
            axes(handles.g5);
            cla;
            hold on;
            plot(X1, H1, 'r');
            plot(X2, H2, 'g');
            plot(X3, H3, 'b');
            axis([0 256 0 max([H1 H2 H3])]);
        end
        
        function updateg4_1(app, handles)
            
            k=handles.img(:,:,1);
            x=size(k);
            x=(1:x(1,2));
            k=k(1,:);
            axes(handles.g4);
            cla;
            plot(x,k,'k');
            
            ImageData = reshape(handles.img, [size(handles.img, 1) * size(handles.img, 2) 1]);
            [H, X] = hist(ImageData, 1:5:256);
            axes(handles.g5);
            cla;
            hold on;
            plot(X, H, 'k');
            axis([0 256 0 max(H)]);
        end
        
    end
    

    % Callbacks that handle component events
    methods (Access = private)

        % Code that executes after component creation
        function Image_processing_GUI_OpeningFcn(app, varargin)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app); %#ok<ASGLU>
            
            % This function has no output args, see OutputFcn.
            % hObject    handle to figure
            % eventdata  reserved - to be defined in a future version of MATLAB
            % handles    structure with handles and user data (see GUIDATA)
            % varargin   command line arguments to Image_processing_GUI (see VARARGIN)
            
            % Choose default command line output for Image_processing_GUI
            handles.output = hObject;
            
            set(handles.save,'Enable','off');
            set(handles.exit,'Enable','off');
            set(handles.reset,'Enable','off');
            set(handles.g1,'Visible','off');
            set(handles.g2,'Visible','off');
            set(handles.g3,'Visible','off');
            set(handles.g4,'Visible','off');
            set(handles.g5,'Visible','off');
            set(handles.n1,'Enable','off');
            set(handles.n2,'Enable','off');
            set(handles.n3,'Enable','off');
            set(handles.n4,'Enable','off');
            set(handles.f1,'Enable','off');
            set(handles.f2,'Enable','off');
            set(handles.f3,'Enable','off');
            set(handles.slider1,'Enable','off');
            set(handles.slider2,'Enable','off');
            set(handles.slider3,'Enable','off');
            set(handles.slider4,'Enable','off');
            set(handles.slider5,'Enable','off');
            set(handles.m1,'Enable','off');
            set(handles.m2,'Enable','off');
            set(handles.m3,'Enable','off');
            set(handles.m4,'Enable','off');
            set(handles.m5,'Enable','off');
            set(handles.m6,'Enable','off');
            set(handles.m7,'Enable','off');
            set(handles.m8,'Enable','off');
            set(handles.m9,'Enable','off');
            set(handles.p1,'Enable','off');
            set(handles.p2,'Enable','off');
            set(handles.p3,'Enable','off');
            set(handles.p4,'Enable','off');
            set(handles.p5,'Enable','off');
            set(handles.invc,'Enable','off');
            set(handles.dia,'Enable','off');
            set(handles.ero,'Enable','off');
            set(handles.strel,'Enable','off');
            set(handles.strels,'Enable','off');
            set(handles.strelv,'Enable','off');
            
            % Update handles structure
            guidata(hObject, handles);
        end

        % Button pushed function: dia
        function dia_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.mori=handles.img;
            str=strel(handles.strel,handles.value);
            handles.mori=imdilate(handles.mori,str);
            axes(handles.g2);
            cla;
            imshow(handles.mori)
            guidata(hObject,handles);
            handles.img=handles.mori;
            mysize=size(handles.img);
            
            guidata(hObject,handles);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: ero
        function ero_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.mori=handles.img;
            str=strel(handles.strel,handles.value);
            handles.mori=imerode(handles.mori,str);
            axes(handles.g2);
            cla;
            imshow(handles.mori)
            guidata(hObject,handles);
            handles.img=handles.mori;
            
            guidata(hObject,handles);
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: exit
        function exit_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            close all;
        end

        % Button pushed function: f1
        function f1_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            h=fspecial('average');
            handles.img=imfilter(handles.img,h,'replicate');
            axes(handles.g2); cla; imshow(handles.img)
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: f2
        function f2_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            hsize=[8 8]; sigma=1.7;
            h=fspecial('gaussian',hsize,sigma);
            handles.img=imfilter(handles.img,h,'replicate');
            axes(handles.g2); cla; imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: f3
        function f3_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            r=medfilt2(handles.img(:,:,1));
            g=medfilt2(handles.img(:,:,2));
            b=medfilt2(handles.img(:,:,3));
            handles.img=cat(3,r,g,b);
            axes(handles.g2); cla; imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: invc
        function invc_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            x=handles.img;
            r=x(:,:,1); r=256-r;
            g=x(:,:,2); g=256-g;
            b=x(:,:,3); b=256-b;
            handles.img=cat(3,r,g,b);
            axes(handles.g2);
            cla;
            imshow(handles.img);
            updateg4(app, handles);
            guidata(hObject,handles);
        end

        % Button pushed function: load
        function load_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            [file path]=uigetfile({'*.jpg';'*.bmp';'*.jpeg';'*.png'}, '打开文件');
            image=[path file];
            handles.file=image;
            if (file==0)
                warndlg('请选择一张图片...') ;
            end
            [fpath, fname, fext]=fileparts(file);
            validex=({'.bmp','.jpg','.jpeg','.png'});
            found=0;
            for (x=1:length(validex))
               if (strcmpi(fext,validex{x}))
                   found=1;
            
            set(handles.save,'Enable','on');
            set(handles.exit,'Enable','on');
            set(handles.reset,'Enable','on');
            set(handles.g1,'Visible','on');
            set(handles.g2,'Visible','on');
            set(handles.g3,'Visible','on');
            set(handles.g4,'Visible','on');
            set(handles.g5,'Visible','on');
            set(handles.n1,'Enable','on');
            set(handles.n2,'Enable','on');
            set(handles.n3,'Enable','on');
            set(handles.n4,'Enable','on');
            set(handles.f1,'Enable','on');
            set(handles.f2,'Enable','on');
            set(handles.f3,'Enable','on');
            set(handles.slider1,'Enable','on');
            set(handles.slider2,'Enable','on');
            set(handles.slider3,'Enable','on');
            set(handles.slider4,'Enable','on');
            set(handles.slider5,'Enable','on');
            set(handles.m1,'Enable','on');
            set(handles.m2,'Enable','on');
            set(handles.m3,'Enable','on');
            set(handles.m4,'Enable','on');
            set(handles.m5,'Enable','on');
            set(handles.m6,'Enable','on');
            set(handles.m7,'Enable','on');
            set(handles.m8,'Enable','on');
            set(handles.m9,'Enable','on');
            set(handles.p1,'Enable','on');
            set(handles.p2,'Enable','on');
            set(handles.p3,'Enable','on');
            set(handles.p4,'Enable','on');
            set(handles.p5,'Enable','on');
            set(handles.invc,'Enable','on');
            set(handles.dia,'Enable','on');
            set(handles.ero,'Enable','on');
            set(handles.strel,'Enable','on');
            set(handles.strels,'Enable','on');
            set(handles.strelv,'Enable','on');
            
            handles.img=imread(image);
            handles.i=imread(image);
            h = waitbar(0,'等待...');
            steps = 100;
            
            for step = 1:steps
                waitbar(step / steps)
            end
            close(h)
            axes(handles.g1);
            cla;
            imshow(handles.img);
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            break;
            end
            end
            if (found==0)
                 errordlg('文件扩展名不正确，请从可用扩展名[.jpg、.jpeg、.bmp、.png]中选择文件','Image Format Error');
            end
            
            set(handles.g3,'Visible','on');
            set(handles.g4,'Visible','on');
            
            % 示波器g3更新
            mysize=size(handles.img);
            if numel(mysize)>2
                r=handles.i(:,:,1);
                g=handles.i(:,:,2);
                b=handles.i(:,:,3);
                x=size(r);
                x=(1:x(1,2));
                r=r(1,:);
                g=g(1,:);
                b=b(1,:);
                axes(handles.g3);
                cla;
                plot(x,r,'r');
                hold on
                plot(x,g,'g');
                plot(x,b,'b');
                set(gca,'xtick',-inf:inf:inf);
                hold off;
            else
                k=handles.img(:,:,1);
                x=size(k);
                x=(1:x(1,2));
                k=k(1,:);
                axes(handles.g3);
                cla;
                plot(x,k,'k');
            end
            
            %示波器g5更新
            mysize=size(handles.img);
            if numel(mysize)>2
                ImageData1 = reshape(handles.img(:,:,1), [size(handles.img, 1) * size(handles.img, 2) 1]);
                ImageData2 = reshape(handles.img(:,:,2), [size(handles.img, 1) * size(handles.img, 2) 1]);
                ImageData3 = reshape(handles.img(:,:,3), [size(handles.img, 1) * size(handles.img, 2) 1]);
                [H1, X1] = hist(ImageData1, 1:5:256);
                [H2, X2] = hist(ImageData2, 1:5:256);
                [H3, X3] = hist(ImageData3, 1:5:256);
                axes(handles.g5);
                cla;
                hold on;
                plot(X1, H1, 'r');
                plot(X2, H2, 'g');
                plot(X3, H3, 'b');
                axis([0 256 0 max([H1 H2 H3])]);
            else
                ImageData = reshape(handles.img, [size(handles.img, 1) * size(handles.img, 2) 1]);
                [H, X] = hist(ImageData, 1:5:256);
                axes(handles.g5);
                cla;
                hold on;
                plot(X, H, 'k');
                axis([0 256 0 max(H)]);
            end
            
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: m1
        function m1_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img=rgb2gray(handles.img);
            end
            handles.img=edge(handles.img,'sobel');
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            updateg4_1(app, handles);
        end

        % Button pushed function: m2
        function m2_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img=rgb2gray(handles.img);
            end
            handles.img=edge(handles.img,'roberts');
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            updateg4_1(app, handles);
        end

        % Button pushed function: m3
        function m3_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img=rgb2gray(handles.img);
            end
            handles.img=edge(handles.img,'prewitt');
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            updateg4_1(app, handles);
        end

        % Button pushed function: m4
        function m4_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img=rgb2gray(handles.img);
            end
            handles.img=edge(handles.img,'log');
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            updateg4_1(app, handles);
        end

        % Button pushed function: m5
        function m5_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img=rgb2gray(handles.img);
            end
            handles.img=edge(handles.img,'canny');
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            updateg4_1(app, handles);
        end

        % Button pushed function: m6
        function m6_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img = rgb2gray(handles.img);
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            updateg4_1(app, handles);
        end

        % Button pushed function: m7
        function m7_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            thresh = graythresh(handles.img);     %自动确定二值化阈值
            handles.img = im2bw(handles.img,thresh);
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            updateg4_1(app, handles);
        end

        % Button pushed function: m8
        function m8_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img=fliplr(handles.img);
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: m9
        function m9_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img=flipud(handles.img);
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: n1
        function n1_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img = imnoise(handles.img,'gaussian');
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: n2
        function n2_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            h = waitbar(0,'等待...');
            steps = 200;
            for step = 1:steps
                waitbar(step / steps)
            end
            close(h)
            handles.img = imnoise(handles.img,'poisson');
                axes(handles.g2); cla; imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: n3
        function n3_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img = imnoise(handles.img,'salt & pepper',0.02);
            axes(handles.g2); cla; imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: n4
        function n4_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img = imnoise(handles.img,'speckle',0.04);
            axes(handles.g2); cla; imshow(handles.img);
            guidata(hObject,handles);
            
            mysize=size(handles.img);
            if numel(mysize)>2
                updateg4(app, handles)
            else
                updateg4_1(app, handles)
            end
        end

        % Button pushed function: p1
        function p1_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            %维纳滤波复原，复原含有噪声和运动模糊的图片
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img = rgb2gray(handles.img);
            end
            
            I=handles.img;
            I=im2double(I);
            LEN=20;%参数设置
            THETA=10;
            PSF=fspecial('motion',LEN,THETA);%产生PSF
            J=imfilter(I,PSF,'conv','circular');%运动模糊
            noise=0.03*randn(size(I));
            K=imadd(J,noise);%添加噪声
            NP=abs(fft2(noise)).^2;
            NPower=sum(NP(:))/prod(size(noise));
            NCORR=fftshift(real(ifft2(NP)));
            IP=abs(fft2(I).^2);
            IPower=sum(IP(:))/prod(size(I));
            ICORR=fftshift(real(ifft2(IP)));
            L=deconvwnr(K,PSF,NCORR,ICORR);
            handles.img=L;
            
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            updateg4_1(app, handles)
        end

        % Button pushed function: p2
        function p2_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            %理想低通滤波器
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img = rgb2gray(handles.img);
            end
            
            I=handles.img;
            I=im2double(I);
            M=2*size(I,1);
            N=2*size(I,2);
            u=-M/2:(M/2-1);
            v=-N/2:(N/2-1);
            [U,V]=meshgrid(u,v);
            D=sqrt(U.^2+V.^2);
            D0=80;
            H=double(D<=D0);
            J=fftshift(fft2(I,size(H,1),size(H,2)));
            K=J.*H;
            L=ifft2(ifftshift(K));
            L=L(1:size(I,1),1:size(I,2));
            handles.img=L;
            
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            updateg4_1(app, handles)
        end

        % Button pushed function: p3
        function p3_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            %巴特沃斯高通滤波器
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img = rgb2gray(handles.img);
            end
            
            I=handles.img;
            I=im2double(I);
            M=2*size(I,1);%滤波器行数
            N=2*size(I,2);%滤波器列数
            u=-M/2:(M/2-1);
            v=-N/2:(N/2-1);
            [U,V]=meshgrid(u,v);
            D=sqrt(U.^2+V.^2);
            D0=30;%截止频率
            n=6;%巴特沃斯滤波器阶数
            H=1./(1+(D0./D).^(2*n));
            J=fftshift(fft2(I,size(H,1),size(H,2)));
            K=J.*H;
            L=ifft2(ifftshift(K));
            L=L(1:size(I,1),1:size(I,2));
            handles.img=L;
            
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            updateg4_1(app, handles)
        end

        % Button pushed function: p4
        function p4_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            %带阻滤波器
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img = rgb2gray(handles.img);
            end
            
            I=handles.img;
            I=imnoise(I,'gaussian',0,0.01);
            I=im2double(I);
            M=2*size(I,1);%滤波器行数
            N=2*size(I,2);%滤波器列数
            u=-M/2:(M/2-1);
            v=-N/2:(N/2-1);
            [U,V]=meshgrid(u,v);
            D=sqrt(U.^2+V.^2);
            D0=50;%滤波器D0
            W=30;%滤波器带宽
            H=double(or(D<(D0-W/2),D>D0+W/2));
            J=fftshift(fft2(I,size(H,1),size(H,2)));
            K=J.*H;
            L=ifft2(ifftshift(K));
            L=L(1:size(I,1),1:size(I,2));
            handles.img=L;
            
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            updateg4_1(app, handles)
        end

        % Button pushed function: p5
        function p5_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            %同态滤波器
            mysize=size(handles.img);
            if numel(mysize)>2
                handles.img = rgb2gray(handles.img);
            end
            
            I=handles.img;
            J=log(im2double(I)+1);
            K=fft2(J);
            n=5;
            D0=0.1*pi;
            rh=0.7;
            rl=0.4;
            [row, column]=size(J);
            for i=1:row
                for j=i:column
                    D1(i,j)=sqrt(i^2+j^2);
                    H(i,j)=rl+(rh/(1+(D0/D1(i,j))^(2*n)));
                end
            end
            L=K.*H;
            M=ifft2(L);
            N=exp(M)-1;
            handles.img=N;
            
            axes(handles.g2);
            cla;
            imshow(handles.img);
            guidata(hObject,handles);
            
            updateg4_1(app, handles)
        end

        % Button pushed function: reset
        function reset_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img=handles.i;
            axes(handles.g2);
            cla;
            imshow(handles.img);
            updateg4(app, handles);
            guidata(hObject,handles);
        end

        % Button pushed function: save
        function save_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            [file path]= uiputfile('*.jpg','Save Image as');
            save=[path file]; imwrite(handles.img,save,'jpg');
        end

        % Value changed function: slider1
        function slider1_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            x=get(hObject,'Value');
            r=handles.img(:,:,1);
            g=handles.img(:,:,2);
            b=handles.img(:,:,3);
            r1=r+x;
            rcon=cat(3,r1,g,b);
            axes(handles.g2);
            cla;
            imshow(rcon)
            handles.img=rcon;
            r=handles.img(:,:,1);
            g=handles.img(:,:,2);
            b=handles.img(:,:,3);
            x=size(r);
            x=(1:x(1,2));
            r=r(1,:);
            g=g(1,:);
            b=b(1,:);
            axes(handles.g4);
            plot(x,r,'r');
            hold on
            plot(x,g,'g');
            plot(x,b,'b');
            hold off;
        end

        % Value changed function: slider2
        function slider2_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            x=get(hObject,'Value');
            r=handles.img(:,:,1);
            g=handles.img(:,:,2); b=handles.img(:,:,3);
            g1=g+x; gcon=cat(3,r,g1,b);
            axes(handles.g2); cla; imshow(gcon)
            handles.img=gcon;
            
            updateg4(app, handles)
        end

        % Value changed function: slider3
        function slider3_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            x=get(hObject,'Value');
            r=handles.img(:,:,1);
            g=handles.img(:,:,2); b=handles.img(:,:,3);
            b1=b+x; bcon=cat(3,r,g,b1);
            axes(handles.g2); cla; imshow(bcon)
            handles.img=bcon;
            updateg4(app, handles)
        end

        % Value changed function: slider4
        function slider4_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            x=get(hObject,'Value');
            img=handles.img;
            img=img+x;
            axes(handles.g2); cla; imshow(img)
            handles.img=img;
            updateg4(app, handles)
        end

        % Value changed function: slider5
        function slider5_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            rrv=(get(hObject,'Value'));
            handles.rot=handles.img;
            handles.rot=imrotate(handles.rot,rrv);
            axes(handles.g2); cla; imshow(handles.rot);
            guidata(hObject,handles)
        end

        % Value changed function: strel
        function strel_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            val=get(hObject,'Value');
            switch val
                    case 1
                    helpdlg('请选择一种结构元素类型','Morphological Operations');
                    case 2
                    handles.strel='diamond';
                    case 3
                    handles.strel='disk';
                    case 4
                    helpdlg('值必须是3的非负倍数','Octagon Structure Help');
                    handles.strel='octagon';
            end
            guidata(hObject,handles);
        end

        % Button pushed function: strels
        function strels_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.img=handles.mori;
            axes(handles.g2);
            cla;
            imshow(handles.img)
            guidata(hObject,handles);
            updateg4(app, handles);
        end

        % Value changed function: strelv
        function strelv_Callback(app, event)
            % Create GUIDE-style callback args - Added by Migration Tool
            [hObject, eventdata, handles] = convertToGUIDECallbackArguments(app, event); %#ok<ASGLU>
            
            
            handles.value=str2num(get(hObject,'String'));
            guidata(hObject,handles);
        end
    end

    % Component initialization
    methods (Access = private)

        % Create UIFigure and components
        function createComponents(app)

            % Create figure1 and hide until all components are created
            app.figure1 = uifigure('Visible', 'off');
            app.figure1.Position = [725 541 997 641];
            app.figure1.Name = 'Image_processing_GUI';
            app.figure1.Resize = 'off';
            app.figure1.HandleVisibility = 'callback';
            app.figure1.Tag = 'figure1';

            % Create g1
            app.g1 = uiaxes(app.figure1);
            app.g1.FontSize = 12.6666666666667;
            app.g1.NextPlot = 'replace';
            app.g1.Tag = 'g1';
            app.g1.Position = [63 240 268 366];

            % Create g2
            app.g2 = uiaxes(app.figure1);
            app.g2.FontSize = 12.6666666666667;
            app.g2.NextPlot = 'replace';
            app.g2.Tag = 'g2';
            app.g2.Position = [315 240 268 366];

            % Create g3
            app.g3 = uiaxes(app.figure1);
            app.g3.XLim = [0 1];
            app.g3.XTick = [];
            app.g3.XTickLabel = '';
            app.g3.FontSize = 11.3333333333333;
            app.g3.NextPlot = 'replace';
            app.g3.Tag = 'g3';
            app.g3.Position = [722 484 265 120];

            % Create g4
            app.g4 = uiaxes(app.figure1);
            app.g4.FontSize = 11.3333333333333;
            app.g4.NextPlot = 'replace';
            app.g4.Tag = 'g4';
            app.g4.Position = [722 368 268 130];

            % Create g5
            app.g5 = uiaxes(app.figure1);
            app.g5.FontSize = 11.3333333333333;
            app.g5.NextPlot = 'replace';
            app.g5.Tag = 'g5';
            app.g5.Position = [722 242 268 130];

            % Create load
            app.load = uibutton(app.figure1, 'push');
            app.load.ButtonPushedFcn = createCallbackFcn(app, @load_Callback, true);
            app.load.Tag = 'load';
            app.load.FontSize = 20;
            app.load.Position = [11.6666666666667 526.777777777778 64 42];
            app.load.Text = '打开';

            % Create exit
            app.exit = uibutton(app.figure1, 'push');
            app.exit.ButtonPushedFcn = createCallbackFcn(app, @exit_Callback, true);
            app.exit.Tag = 'exit';
            app.exit.FontSize = 20;
            app.exit.Position = [11.6666666666667 465.333333333333 64 42];
            app.exit.Text = '退出';

            % Create save
            app.save = uibutton(app.figure1, 'push');
            app.save.ButtonPushedFcn = createCallbackFcn(app, @save_Callback, true);
            app.save.Tag = 'save';
            app.save.FontSize = 20;
            app.save.Position = [11.6666666666667 403.888888888889 64 42];
            app.save.Text = '保存';

            % Create reset
            app.reset = uibutton(app.figure1, 'push');
            app.reset.ButtonPushedFcn = createCallbackFcn(app, @reset_Callback, true);
            app.reset.Tag = 'reset';
            app.reset.FontSize = 20;
            app.reset.Position = [11.6666666666667 342.444444444444 64 42];
            app.reset.Text = '清除';

            % Create text2
            app.text2 = uilabel(app.figure1);
            app.text2.Tag = 'text2';
            app.text2.HorizontalAlignment = 'center';
            app.text2.VerticalAlignment = 'top';
            app.text2.WordWrap = 'on';
            app.text2.FontSize = 26.6666666666667;
            app.text2.FontColor = [1 0 0];
            app.text2.Position = [149.571428571429 598.333333333333 108.190476190476 35.7777777777778];
            app.text2.Text = '原始图片';

            % Create text3
            app.text3 = uilabel(app.figure1);
            app.text3.Tag = 'text3';
            app.text3.HorizontalAlignment = 'center';
            app.text3.VerticalAlignment = 'top';
            app.text3.WordWrap = 'on';
            app.text3.FontSize = 26.6666666666667;
            app.text3.FontColor = [1 0 0];
            app.text3.Position = [389.571428571429 603.777777777778 108.190476190476 30.3333333333333];
            app.text3.Text = '效果预览';

            % Create text4
            app.text4 = uilabel(app.figure1);
            app.text4.Tag = 'text4';
            app.text4.HorizontalAlignment = 'center';
            app.text4.VerticalAlignment = 'top';
            app.text4.WordWrap = 'on';
            app.text4.FontSize = 26.6666666666667;
            app.text4.FontColor = [1 0 0];
            app.text4.Position = [820.047619047619 606.111111111111 91.4285714285714 28];
            app.text4.Text = '示波器';

            % Create uibuttongroup1
            app.uibuttongroup1 = uibuttongroup(app.figure1);
            app.uibuttongroup1.Title = '添加噪波';
            app.uibuttongroup1.Tag = 'uibuttongroup1';
            app.uibuttongroup1.FontSize = 16;
            app.uibuttongroup1.Position = [696 23 117 224];

            % Create n1
            app.n1 = uibutton(app.uibuttongroup1, 'push');
            app.n1.ButtonPushedFcn = createCallbackFcn(app, @n1_Callback, true);
            app.n1.Tag = 'n1';
            app.n1.FontSize = 16;
            app.n1.Position = [15.4761904761905 162 85.3333333333333 28];
            app.n1.Text = '高斯噪波';

            % Create n3
            app.n3 = uibutton(app.uibuttongroup1, 'push');
            app.n3.ButtonPushedFcn = createCallbackFcn(app, @n3_Callback, true);
            app.n3.Tag = 'n3';
            app.n3.FontSize = 16;
            app.n3.Position = [15.4761904761905 74.888888888889 85.3333333333333 28];
            app.n3.Text = '椒盐噪波';

            % Create n4
            app.n4 = uibutton(app.uibuttongroup1, 'push');
            app.n4.ButtonPushedFcn = createCallbackFcn(app, @n4_Callback, true);
            app.n4.Tag = 'n4';
            app.n4.FontSize = 16;
            app.n4.Position = [15.4761904761905 31.3333333333334 85.3333333333333 28];
            app.n4.Text = '斑点噪波';

            % Create n2
            app.n2 = uibutton(app.uibuttongroup1, 'push');
            app.n2.ButtonPushedFcn = createCallbackFcn(app, @n2_Callback, true);
            app.n2.Tag = 'n2';
            app.n2.FontSize = 16;
            app.n2.Position = [15.4761904761905 118.444444444444 85.3333333333333 28];
            app.n2.Text = '泊松噪波';

            % Create uibuttongroup2
            app.uibuttongroup2 = uibuttongroup(app.figure1);
            app.uibuttongroup2.Title = '空间滤波器/去噪';
            app.uibuttongroup2.Tag = 'uibuttongroup2';
            app.uibuttongroup2.FontSize = 16;
            app.uibuttongroup2.Position = [396 79 133 168];

            % Create f2
            app.f2 = uibutton(app.uibuttongroup2, 'push');
            app.f2.ButtonPushedFcn = createCallbackFcn(app, @f2_Callback, true);
            app.f2.Tag = 'f2';
            app.f2.FontSize = 16;
            app.f2.Position = [26.1428571428571 60.8888888888889 85.3333333333333 28];
            app.f2.Text = '高斯滤波';

            % Create f1
            app.f1 = uibutton(app.uibuttongroup2, 'push');
            app.f1.ButtonPushedFcn = createCallbackFcn(app, @f1_Callback, true);
            app.f1.Tag = 'f1';
            app.f1.FontSize = 16;
            app.f1.Position = [26.1428571428571 104.444444444444 85.3333333333333 28];
            app.f1.Text = '均值滤波';

            % Create f3
            app.f3 = uibutton(app.uibuttongroup2, 'push');
            app.f3.ButtonPushedFcn = createCallbackFcn(app, @f3_Callback, true);
            app.f3.Tag = 'f3';
            app.f3.FontSize = 16;
            app.f3.Position = [26.1428571428571 17.3333333333333 85.3333333333333 28];
            app.f3.Text = '中值滤波';

            % Create uibuttongroup4
            app.uibuttongroup4 = uibuttongroup(app.figure1);
            app.uibuttongroup4.Title = '基础功能';
            app.uibuttongroup4.Tag = 'uibuttongroup4';
            app.uibuttongroup4.FontSize = 16;
            app.uibuttongroup4.Position = [262 79 117 168];

            % Create m8
            app.m8 = uibutton(app.uibuttongroup4, 'push');
            app.m8.ButtonPushedFcn = createCallbackFcn(app, @m8_Callback, true);
            app.m8.Tag = 'm8';
            app.m8.FontSize = 16;
            app.m8.Position = [15.4761904761905 106.777777777778 85.3333333333333 28];
            app.m8.Text = '左右翻转';

            % Create m9
            app.m9 = uibutton(app.uibuttongroup4, 'push');
            app.m9.ButtonPushedFcn = createCallbackFcn(app, @m9_Callback, true);
            app.m9.Tag = 'm9';
            app.m9.FontSize = 16;
            app.m9.Position = [15.4761904761905 63.2222222222222 85.3333333333333 28];
            app.m9.Text = '上下翻转';

            % Create m6
            app.m6 = uibutton(app.uibuttongroup4, 'push');
            app.m6.ButtonPushedFcn = createCallbackFcn(app, @m6_Callback, true);
            app.m6.Tag = 'm6';
            app.m6.FontSize = 16;
            app.m6.Position = [15.4761904761905 19.6666666666667 85.3333333333333 28];
            app.m6.Text = '灰度';

            % Create uibuttongroup5
            app.uibuttongroup5 = uibuttongroup(app.figure1);
            app.uibuttongroup5.Title = '边缘检测';
            app.uibuttongroup5.Tag = 'uibuttongroup5';
            app.uibuttongroup5.FontSize = 16;
            app.uibuttongroup5.Position = [581 386 133 225];

            % Create m1
            app.m1 = uibutton(app.uibuttongroup5, 'push');
            app.m1.ButtonPushedFcn = createCallbackFcn(app, @m1_Callback, true);
            app.m1.Tag = 'm1';
            app.m1.FontSize = 16;
            app.m1.Position = [23.0952380952381 168.222222222222 85.3333333333333 28];
            app.m1.Text = 'Sobel';

            % Create m2
            app.m2 = uibutton(app.uibuttongroup5, 'push');
            app.m2.ButtonPushedFcn = createCallbackFcn(app, @m2_Callback, true);
            app.m2.Tag = 'm2';
            app.m2.FontSize = 16;
            app.m2.Position = [23.0952380952381 130.888888888889 85.3333333333333 28];
            app.m2.Text = 'Roberts';

            % Create m3
            app.m3 = uibutton(app.uibuttongroup5, 'push');
            app.m3.ButtonPushedFcn = createCallbackFcn(app, @m3_Callback, true);
            app.m3.Tag = 'm3';
            app.m3.FontSize = 16;
            app.m3.Position = [23.0952380952381 93.5555555555557 85.3333333333333 28];
            app.m3.Text = 'Prewitt';

            % Create m4
            app.m4 = uibutton(app.uibuttongroup5, 'push');
            app.m4.ButtonPushedFcn = createCallbackFcn(app, @m4_Callback, true);
            app.m4.Tag = 'm4';
            app.m4.FontSize = 16;
            app.m4.Position = [23.0952380952381 56.2222222222224 85.3333333333333 28];
            app.m4.Text = 'LOG';

            % Create m5
            app.m5 = uibutton(app.uibuttongroup5, 'push');
            app.m5.ButtonPushedFcn = createCallbackFcn(app, @m5_Callback, true);
            app.m5.Tag = 'm5';
            app.m5.FontSize = 16;
            app.m5.Position = [23.0952380952381 18.8888888888891 85.3333333333333 28];
            app.m5.Text = 'Canny';

            % Create uibuttongroup6
            app.uibuttongroup6 = uibuttongroup(app.figure1);
            app.uibuttongroup6.Title = '形态学操作';
            app.uibuttongroup6.Tag = 'uibuttongroup6';
            app.uibuttongroup6.FontSize = 16;
            app.uibuttongroup6.Position = [830 23 149 224];

            % Create strels
            app.strels = uibutton(app.uibuttongroup6, 'push');
            app.strels.ButtonPushedFcn = createCallbackFcn(app, @strels_Callback, true);
            app.strels.Tag = 'strels';
            app.strels.FontSize = 16;
            app.strels.Position = [87.0952380952381 28.2222222222223 42.6666666666667 28];
            app.strels.Text = '设置';

            % Create dia
            app.dia = uibutton(app.uibuttongroup6, 'push');
            app.dia.ButtonPushedFcn = createCallbackFcn(app, @dia_Callback, true);
            app.dia.Tag = 'dia';
            app.dia.FontSize = 16;
            app.dia.Position = [32.2380952380952 158.888888888889 85.3333333333333 28];
            app.dia.Text = '膨胀';

            % Create ero
            app.ero = uibutton(app.uibuttongroup6, 'push');
            app.ero.ButtonPushedFcn = createCallbackFcn(app, @ero_Callback, true);
            app.ero.Tag = 'ero';
            app.ero.FontSize = 16;
            app.ero.Position = [32.2380952380952 115.333333333333 85.3333333333333 28];
            app.ero.Text = '腐蚀';

            % Create strelv
            app.strelv = uieditfield(app.uibuttongroup6, 'text');
            app.strelv.ValueChangedFcn = createCallbackFcn(app, @strelv_Callback, true);
            app.strelv.Tag = 'strelv';
            app.strelv.HorizontalAlignment = 'center';
            app.strelv.FontSize = 16;
            app.strelv.Position = [19.2857142857143 28.2222222222223 42.6666666666667 28];

            % Create strel
            app.strel = uidropdown(app.uibuttongroup6);
            app.strel.Items = {'结构元素类型', 'diamond', 'disk', 'otcagon'};
            app.strel.ValueChangedFcn = createCallbackFcn(app, @strel_Callback, true);
            app.strel.Tag = 'strel';
            app.strel.FontSize = 16;
            app.strel.BackgroundColor = [1 1 1];
            app.strel.Position = [16.2380952380952 71.7777777777779 117.333333333333 28];
            app.strel.Value = '结构元素类型';

            % Create slider5
            app.slider5 = uislider(app.figure1);
            app.slider5.Limits = [-180 180];
            app.slider5.MajorTicks = [];
            app.slider5.ValueChangedFcn = createCallbackFcn(app, @slider5_Callback, true);
            app.slider5.MinorTicks = [];
            app.slider5.Tag = 'slider5';
            app.slider5.FontSize = 10.6666666666667;
            app.slider5.Position = [329.380952380952 32.8888888888889 191.238095238095 3];

            % Create text9
            app.text9 = uilabel(app.figure1);
            app.text9.Tag = 'text9';
            app.text9.HorizontalAlignment = 'center';
            app.text9.VerticalAlignment = 'top';
            app.text9.WordWrap = 'on';
            app.text9.FontSize = 16;
            app.text9.Position = [273 28.2222222222222 32 28];
            app.text9.Text = '旋转';

            % Create uibuttongroup3
            app.uibuttongroup3 = uibuttongroup(app.figure1);
            app.uibuttongroup3.Title = '颜色/亮度调整';
            app.uibuttongroup3.Tag = 'uibuttongroup3';
            app.uibuttongroup3.FontSize = 16;
            app.uibuttongroup3.Position = [21 23 224 224];

            % Create slider1
            app.slider1 = uislider(app.uibuttongroup3);
            app.slider1.Limits = [-200 200];
            app.slider1.MajorTicks = [];
            app.slider1.ValueChangedFcn = createCallbackFcn(app, @slider1_Callback, true);
            app.slider1.MinorTicks = [];
            app.slider1.Tag = 'slider1';
            app.slider1.FontSize = 10.6666666666667;
            app.slider1.Position = [47.4761904761905 160.444444444444 160 3];

            % Create slider2
            app.slider2 = uislider(app.uibuttongroup3);
            app.slider2.Limits = [-200 200];
            app.slider2.MajorTicks = [];
            app.slider2.ValueChangedFcn = createCallbackFcn(app, @slider2_Callback, true);
            app.slider2.MinorTicks = [];
            app.slider2.Tag = 'slider2';
            app.slider2.FontSize = 10.6666666666667;
            app.slider2.Position = [47.4761904761905 116.888888888889 160 3];

            % Create slider3
            app.slider3 = uislider(app.uibuttongroup3);
            app.slider3.Limits = [-200 200];
            app.slider3.MajorTicks = [];
            app.slider3.ValueChangedFcn = createCallbackFcn(app, @slider3_Callback, true);
            app.slider3.MinorTicks = [];
            app.slider3.Tag = 'slider3';
            app.slider3.FontSize = 10.6666666666667;
            app.slider3.Position = [47.4761904761905 73.3333333333334 160 3];

            % Create text5
            app.text5 = uilabel(app.uibuttongroup3);
            app.text5.Tag = 'text5';
            app.text5.HorizontalAlignment = 'center';
            app.text5.VerticalAlignment = 'top';
            app.text5.WordWrap = 'on';
            app.text5.FontSize = 16;
            app.text5.Position = [10.1428571428571 155 32 28];
            app.text5.Text = 'R';

            % Create text6
            app.text6 = uilabel(app.uibuttongroup3);
            app.text6.Tag = 'text6';
            app.text6.HorizontalAlignment = 'center';
            app.text6.VerticalAlignment = 'top';
            app.text6.WordWrap = 'on';
            app.text6.FontSize = 16;
            app.text6.Position = [10.1428571428571 111.444444444444 32 28];
            app.text6.Text = 'G';

            % Create text7
            app.text7 = uilabel(app.uibuttongroup3);
            app.text7.Tag = 'text7';
            app.text7.HorizontalAlignment = 'center';
            app.text7.VerticalAlignment = 'top';
            app.text7.WordWrap = 'on';
            app.text7.FontSize = 16;
            app.text7.Position = [10.1428571428571 67.8888888888889 32 28];
            app.text7.Text = 'B';

            % Create slider4
            app.slider4 = uislider(app.uibuttongroup3);
            app.slider4.Limits = [-150 150];
            app.slider4.MajorTicks = [];
            app.slider4.ValueChangedFcn = createCallbackFcn(app, @slider4_Callback, true);
            app.slider4.MinorTicks = [];
            app.slider4.Tag = 'slider4';
            app.slider4.FontSize = 10.6666666666667;
            app.slider4.Position = [47.4761904761905 29.7777777777778 160 3];

            % Create text8
            app.text8 = uilabel(app.uibuttongroup3);
            app.text8.Tag = 'text8';
            app.text8.HorizontalAlignment = 'center';
            app.text8.VerticalAlignment = 'top';
            app.text8.WordWrap = 'on';
            app.text8.FontSize = 16;
            app.text8.Position = [10.1428571428571 24.3333333333334 32 28];
            app.text8.Text = '亮度';

            % Create uibuttongroup7
            app.uibuttongroup7 = uibuttongroup(app.figure1);
            app.uibuttongroup7.Tag = 'uibuttongroup7';
            app.uibuttongroup7.FontSize = 10.6666666666667;
            app.uibuttongroup7.Position = [583 255 133 126];

            % Create invc
            app.invc = uibutton(app.uibuttongroup7, 'push');
            app.invc.ButtonPushedFcn = createCallbackFcn(app, @invc_Callback, true);
            app.invc.Tag = 'invc';
            app.invc.FontSize = 16;
            app.invc.Position = [23.8571428571429 50.7777777777778 85.3333333333333 28];
            app.invc.Text = '反色';

            % Create m7
            app.m7 = uibutton(app.uibuttongroup7, 'push');
            app.m7.ButtonPushedFcn = createCallbackFcn(app, @m7_Callback, true);
            app.m7.Tag = 'm7';
            app.m7.FontSize = 16;
            app.m7.Position = [23.8571428571429 88.1111111111111 85.3333333333333 28];
            app.m7.Text = '二值化';

            % Create p1
            app.p1 = uibutton(app.uibuttongroup7, 'push');
            app.p1.ButtonPushedFcn = createCallbackFcn(app, @p1_Callback, true);
            app.p1.Tag = 'p1';
            app.p1.BackgroundColor = [0.635294117647059 0.0784313725490196 0.184313725490196];
            app.p1.FontSize = 16;
            app.p1.FontColor = [0.941176470588235 0.941176470588235 0.941176470588235];
            app.p1.Position = [19.2857142857143 13.4444444444444 96 28];
            app.p1.Text = '维纳滤波复原';

            % Create uibuttongroup8
            app.uibuttongroup8 = uibuttongroup(app.figure1);
            app.uibuttongroup8.Title = '频域滤波';
            app.uibuttongroup8.Tag = 'uibuttongroup8';
            app.uibuttongroup8.FontSize = 16;
            app.uibuttongroup8.Position = [546 23 133 224];

            % Create p2
            app.p2 = uibutton(app.uibuttongroup8, 'push');
            app.p2.ButtonPushedFcn = createCallbackFcn(app, @p2_Callback, true);
            app.p2.Tag = 'p2';
            app.p2.FontSize = 16;
            app.p2.Position = [24.6190476190476 161.222222222222 85.3333333333333 28];
            app.p2.Text = '低通滤波';

            % Create p3
            app.p3 = uibutton(app.uibuttongroup8, 'push');
            app.p3.ButtonPushedFcn = createCallbackFcn(app, @p3_Callback, true);
            app.p3.Tag = 'p3';
            app.p3.FontSize = 16;
            app.p3.Position = [24.6190476190476 117.666666666667 85.3333333333333 28];
            app.p3.Text = '高通滤波';

            % Create p4
            app.p4 = uibutton(app.uibuttongroup8, 'push');
            app.p4.ButtonPushedFcn = createCallbackFcn(app, @p4_Callback, true);
            app.p4.Tag = 'p4';
            app.p4.FontSize = 16;
            app.p4.Position = [24.6190476190476 74.1111111111111 85.3333333333333 28];
            app.p4.Text = '带阻滤波';

            % Create p5
            app.p5 = uibutton(app.uibuttongroup8, 'push');
            app.p5.ButtonPushedFcn = createCallbackFcn(app, @p5_Callback, true);
            app.p5.Tag = 'p5';
            app.p5.FontSize = 16;
            app.p5.Position = [24.6190476190476 30.5555555555556 85.3333333333333 28];
            app.p5.Text = '同态滤波';

            % Show the figure after all components are created
            app.figure1.Visible = 'on';
        end
    end

    % App creation and deletion
    methods (Access = public)

        % Construct app
        function app = Image_processing_GUI_App(varargin)

            runningApp = getRunningApp(app);

            % Check for running singleton app
            if isempty(runningApp)

                % Create UIFigure and components
                createComponents(app)

                % Register the app with App Designer
                registerApp(app, app.figure1)

                % Execute the startup function
                runStartupFcn(app, @(app)Image_processing_GUI_OpeningFcn(app, varargin{:}))
            else

                % Focus the running singleton app
                figure(runningApp.figure1)

                app = runningApp;
            end

            if nargout == 0
                clear app
            end
        end

        % Code that executes before app deletion
        function delete(app)

            % Delete UIFigure when app is deleted
            delete(app.figure1)
        end
    end
end
```

