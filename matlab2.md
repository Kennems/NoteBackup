```matlab
% 总GUI函数
function audio_gui()
    % 定义全局变量
    global fs1;
    global player1;
    global speaker_audio;
    global speaker_audio_samples;
    global speaker_frequency_resolution;
    global fft_speaker_audio;
    global abs_resampled_fft_speaker_audio;
    
    global fs2;
    global player2;
    global mix_audio;
    global mix_audio_samples;
    global mix_frequency_resolution;
    global fft_mix_audio;
    
    global fs3;
    global player3;
    global denoised_audio;
    
    %背景声
    global fs4;
    global player4;
    global background_audio;
    
    % 创建 GUI 窗口
    f = figure('Name', '音频去噪系统', 'NumberTitle', 'off', 'Position', [100, 100, 1400, 710]);
    
    
    % 对窗口进行个性化设置
    % 设置背景颜色为白色
%     set(f, 'Color', 'r');
    % 设置窗口图标
%     icon = imread('audio_icon.png');
%     set(f, 'Icon', icon);
    % 获取用户选择的字体和大小
%     control_font = uisetfont();
    % 获取字体名称和大小
%     font_name = control_font.FontName;
%     font_size = control_font.FontSize;
    % 设置控件字体和大小
%     set(findall(f, 'Type', 'uicontrol'), 'FontName', font_name, 'FontSize', font_size);
    % 设置控件颜色
    button_color = [0, 0.5, 1];
    set(findall(f, 'Type', 'pushbutton'), 'BackgroundColor', button_color);
%     set(findall(f, 'Type', 'uicontrol'), 'FontName', font_name, 'FontSize', font_size);
    % 设置文本框边框样式
    set(findall(f, 'Type', 'uicontrol', 'Style', 'text'), 'HorizontalAlignment', 'left', 'BackgroundColor', 'w', 'BorderType', 'none');
    % 窗口居中显示
    movegui(f, 'center');

    % 创建主讲人音频播放和暂停按钮
    uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '选择主讲人音频', 'Position', [50, 670, 120, 30], 'Callback', @(~,~) select_audio(1));
    %选择混合后音频
    uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '选择混合后人音频', 'Position', [50, 440, 120, 30], 'Callback', @(~,~) select_audio(2));

    % 创建仿真按钮
    uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '开始仿真', 'Position', [50, 210, 120, 30], 'Callback', @denoise_audio);
    
    % 创建背景音乐播放和暂停按钮
%     uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '播放背景音乐', 'Position', [180, 100, 120, 30], 'Callback', @(~,~) play_audio(player4));
%     uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '暂停播放', 'Position', [180, 60, 70, 30], 'Callback', @(~,~) pause_audio(player4), 'Enable', 'on', 'Tag', 'pause_button_4');
    
    % 创建退出按钮
    uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '退出', 'Position', [1300, 30, 70, 30], 'Callback', @close_gui);
    uicontrol('Parent', f, 'Style', 'text', 'String', '退出系统，并关闭所有正在播放的声音', 'Position', [1280, 1, 120, 30], 'FontSize', 8);
    
    %时域波形图
    ax1 = axes('Units', 'pixels', 'Position', [330, 530, 320, 150], 'XTick', [], 'YTick', []);
    ax2 = axes('Units', 'pixels', 'Position', [330, 310, 320, 150], 'XTick', [], 'YTick', []);
    ax3 = axes('Units', 'pixels', 'Position', [330, 70, 320, 150], 'XTick', [], 'YTick', []);
    %幅度谱
    ax4 = axes('Units', 'pixels', 'Position', [680, 530, 320, 150], 'XTick', [], 'YTick', []);
    ax5 = axes('Units', 'pixels', 'Position', [680, 310, 320, 150], 'XTick', [], 'YTick', []);
    ax6 = axes('Units', 'pixels', 'Position', [680, 70, 320, 150], 'XTick', [], 'YTick', []);
    %相位谱
    ax7 = axes('Units', 'pixels', 'Position', [1030, 530, 320, 150], 'XTick', [], 'YTick', []);
    ax8 = axes('Units', 'pixels', 'Position', [1030, 310, 320, 150], 'XTick', [], 'YTick', []);
    ax9 = axes('Units', 'pixels', 'Position', [1030, 70, 320, 150], 'XTick', [], 'YTick', []);
    
    %显示去噪后音频
    title(ax1, '音频时域波形');
    xlabel(ax1, '采样点');
    ylabel(ax1, '幅值');
    
    title(ax4, '幅度谱');
    xlabel(ax4, '采样点');
    ylabel(ax4, '幅值');
    
    title(ax7, '相位谱');
    xlabel(ax7, '采样点');
    ylabel(ax7, '幅值');
    
    title(ax2, '音频时域波形');
    xlabel(ax2, '采样点');
    ylabel(ax2, '幅值');
    
    title(ax5, '幅度谱');
    xlabel(ax5, '采样点');
    ylabel(ax5, '幅值');
    
    title(ax8, '相位谱');
    xlabel(ax8, '采样点');
    ylabel(ax8, '幅值');
    
    title(ax3, '降噪后音频时域波形');
    xlabel(ax3, '采样点');
    ylabel(ax3, '幅值');
    
    title(ax6, '幅度谱');
    xlabel(ax6, '采样点');
    ylabel(ax6, '幅值');

    title(ax9, '相位谱');
    xlabel(ax9, '采样点');
    ylabel(ax9, '幅值');
    
    %选择音频文件
    function select_audio(flag)
        if(flag==1)
            [filename,pathname]=uigetfile({'*.wav;*.m4a;*.mp3;*.mov'},'选择语音信号');
            file1=[pathname,filename];
            [speaker_audio, fs1] = audioread(file1);
            player1 = audioplayer(speaker_audio, fs1);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '显示主讲人音频', 'Position', [50, 590, 120, 30], 'Callback', @show_speaker_audio);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '播放主讲人音频', 'Position', [50, 630, 120, 30], 'Callback', @(~,~) play_audio(1));
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '暂停播放', 'Position', [180, 630, 70, 30], 'Callback', @(~,~) pause_audio(1), 'Enable', 'on', 'Tag', 'pause_button_1');
            %计算后续需要的参数
            resampled_ratio = 1;
            speaker_audio_samples = length(speaker_audio);
            fft_speaker_audio=fft(speaker_audio);
            resampled_fft_speaker_audio = resample(fft_speaker_audio, floor(fs1/resampled_ratio), fs1);
            abs_resampled_fft_speaker_audio = abs(resampled_fft_speaker_audio);
            speaker_time_total_length=(speaker_audio_samples-1)*fs1;
            speaker_frequency_resolution = 1/speaker_time_total_length;
        elseif(flag==2)
            [filename,pathname]=uigetfile({'*.wav;*.m4a;*.mp3;*.mov'},'选择语音信号');
            file2=[pathname,filename];
            [mix_audio, fs2] = audioread(file2);
            player2 = audioplayer(mix_audio, fs2);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '播放混合后音频', 'Position', [50, 400, 120, 30], 'Callback', @(~,~) play_audio(2));
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '显示混合音频', 'Position', [50, 360, 120, 30], 'Callback', @show_mix_audio);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '暂停播放', 'Position', [180, 400, 70, 30], 'Callback', @(~,~) pause_audio(2), 'Enable', 'on', 'Tag', 'pause_button_2');
            mix_audio_samples = length(mix_audio);
            fft_mix_audio=fft(mix_audio);
            mix_time_total_length=(mix_audio_samples-1)*fs2;
            mix_frequency_resolution = 1/mix_time_total_length;
        end
    end
    
    %播放音频
    function play_audio(flag)
        if(flag==1)
            % 创建进度条和文本标签
            h = uicontrol('Parent', f, 'Style', 'slider', 'Position', [50 560 200 20], 'Min', 0, 'Max', 1, 'Value', 0, 'SliderStep', [0.01,0.1]);
            time_label = uicontrol('Parent', f, 'Style', 'text', 'Position', [50, 530, 100, 20], 'String', '00:00 / 00:00');
            remaining_label = uicontrol('Parent', f, 'Style', 'text', 'Position', [150, 530, 100, 20], 'String', '');
            % 更新进度条位置和文本标签的回调函数
            set(player1, 'TimerFcn', {@update_progress_bar, h, time_label, remaining_label});
            % 添加拖动条回调函数，用于更改播放位置
            set(h, 'Callback', {@update_player_position, player1});
            play(player1);
        elseif(flag==2)
            % 创建进度条和文本标签
            h = uicontrol('Parent', f, 'Style', 'slider', 'Position', [50 320 200 20], 'Min', 0, 'Max', 1, 'Value', 0, 'SliderStep', [0.01,0.1]);
            time_label = uicontrol('Parent', f, 'Style', 'text', 'Position', [50, 290, 100, 20], 'String', '00:00 / 00:00');
            remaining_label = uicontrol('Parent', f, 'Style', 'text', 'Position', [150, 290, 100, 20], 'String', '');
            % 更新进度条位置和文本标签的回调函数
            set(player2, 'TimerFcn', {@update_progress_bar, h, time_label, remaining_label});
            % 添加拖动条回调函数，用于更改播放位置
            set(h, 'Callback', {@update_player_position, player2});
            play(player2);
        elseif(flag==3)
            % 创建进度条和文本标签
            h = uicontrol('Parent', f, 'Style', 'slider', 'Position', [50 30 200 20], 'Min', 0, 'Max', 1, 'Value', 0, 'SliderStep', [0.01,0.1]);
            time_label = uicontrol('Parent', f, 'Style', 'text', 'Position', [50, 0, 100, 20], 'String', '00:00 / 00:00');
            remaining_label = uicontrol('Parent', f, 'Style', 'text', 'Position', [150, 0, 100, 20], 'String', '');
            % 更新进度条位置和文本标签的回调函数
            set(player3, 'TimerFcn', {@update_progress_bar, h, time_label, remaining_label});
            % 添加拖动条回调函数，用于更改播放位置
            set(h, 'Callback', {@update_player_position, player3});
            play(player3);
        end
    end

    % 更新进度条位置和文本标签的回调函数
    function update_progress_bar(player, ~, h, time_label, remaining_label)
        % 获取音频总时长以及当前播放的时间
        total_duration = player.TotalSamples/player.SampleRate;
        current_time = player.CurrentSample/player.SampleRate;
        % 计算当前播放进度并更新进度条位置
        progress = current_time/total_duration;
        set(h, 'Value', progress);
        % 格式化已播放时长和总时长并将其显示在文本标签上
        time_str = format_time(current_time);
        total_str = format_time(total_duration);
        set(time_label, 'String', [time_str, ' / ', total_str]);
        % 计算并显示剩余时间
        remaining_time = total_duration - current_time;
        remaining_str = ['-', format_time(remaining_time)];
        set(remaining_label, 'String', remaining_str);
    end

    % 格式化时间的函数
    function time_str = format_time(time)
        minutes = floor(time/60);
        seconds = mod(round(time), 60);
        time_str = sprintf('%02d:%02d', minutes, seconds);
    end

    % 更新播放器位置的回调函数
    function update_player_position(h, ~, player)
        % 获取当前拖动条值以及音频总时长
        value = get(h, 'Value');
        total_duration = player.TotalSamples/player.SampleRate;
        % 将拖动条值转换为对应的播放时间
        current_time = value * total_duration;
        % 将播放器位置设为当前播放时间
        start_sample = round(current_time * player.SampleRate);
        end_sample = player.TotalSamples;
        stop(player);
        play(player, [start_sample, end_sample]);
    end

    % 暂停正在播放的音频
    function pause_audio(flag)
        if(flag==1)
            pause(player1);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '继续播放', 'Position', [180, 630, 70, 30], 'Callback', @(~,~) replay_audio(1), 'Enable', 'on', 'Tag', 'pause_button_1');
        elseif(flag==2)
            pause(player2);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '继续播放', 'Position', [180, 400, 70, 30], 'Callback', @(~,~) replay_audio(2), 'Enable', 'on', 'Tag', 'pause_button_2');
        elseif(flag==3)
            pause(player3);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '继续播放', 'Position', [50, 60, 70, 30], 'Callback', @(~,~) replay_audio(3), 'Enable', 'on', 'Tag', 'pause_button_3');
        end
    end

    % 暂停正在播放的音频
    function replay_audio(flag)
        if(flag==1)
            resume(player1);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '暂停播放', 'Position', [180, 630, 70, 30], 'Callback', @(~,~) pause_audio(1), 'Enable', 'on', 'Tag', 'pause_button_1');
        elseif(flag==2)
            resume(player2);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '暂停播放', 'Position', [180, 400, 70, 30], 'Callback', @(~,~) pause_audio(2), 'Enable', 'on', 'Tag', 'pause_button_2');
        elseif(flag==3)
            resume(player3);
            uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '暂停播放', 'Position', [50, 60, 70, 30], 'Callback', @(~,~) pause_audio(3), 'Enable', 'on', 'Tag', 'pause_button_3');
        end
    end
    
    % 退出程序
    function close_gui(~, ~)
        % 暂停正在播放的音频并关闭所有打开的窗口
        %         clc;
        if (isempty(player1)==false)
            stop(player1);
        end
        if (isempty(player2)==false)
            stop(player2);
        end
        if (isempty(player3)==false)
            stop(player3);
        end
        clear sound;
        close all;
        close(gcf)
    end
    % 显示主讲人音频信息
    function show_speaker_audio(~, ~)
        resampled_ratio = 1;
        resampled_speaker_audio = resample(speaker_audio, floor(fs1/resampled_ratio), fs1);
        speaker_audio_samples = length(speaker_audio);
        Ts=1/fs1;
        speaker_time_total_length=(speaker_audio_samples-1)*Ts;
        speaker_time_axis = 0:Ts:speaker_time_total_length;
        resampled_speaker_time_axis = resample(speaker_time_axis, floor(fs1/resampled_ratio), fs1);
        plot(ax1,resampled_speaker_time_axis,resampled_speaker_audio);
        set(gca,'FontSize',6);
        fft_speaker_audio=fft(speaker_audio);
        resampled_fft_speaker_audio = resample(fft_speaker_audio, floor(fs1/resampled_ratio), fs1);
        speaker_frequency_resolution = 1/speaker_time_total_length;
        speaker_frequency_axis = 0:speaker_frequency_resolution:(speaker_audio_samples-1)*speaker_frequency_resolution;
        resampled_speaker_frequency_axis = resample(speaker_frequency_axis, floor(fs1/resampled_ratio), fs1);
        abs_resampled_fft_speaker_audio = abs(resampled_fft_speaker_audio);
        plot(ax4,resampled_speaker_frequency_axis,abs_resampled_fft_speaker_audio);
        set(gca,'FontSize',6);
        angle_resampled_fft_speaker_audio = angle(resampled_fft_speaker_audio);
        plot(ax7,resampled_speaker_frequency_axis,angle_resampled_fft_speaker_audio);
        set(gca,'FontSize',6);
    end

    function show_mix_audio(~, ~)
        resampled_ratio = 1;
        resampled_mix_audio = resample(mix_audio, floor(fs2/resampled_ratio), fs2);
        mix_audio_samples = length(mix_audio);
        Ts=1/fs2;
        mix_time_total_length=(mix_audio_samples-1)*Ts;
        mix_time_axis = 0:Ts:mix_time_total_length;
        resampled_mix_time_axis = resample(mix_time_axis, floor(fs2/resampled_ratio), fs2);
        plot(ax2,resampled_mix_time_axis,resampled_mix_audio);
        set(gca,'FontSize',6);
        fft_mix_audio=fft(mix_audio);
        resampled_fft_mix_audio = resample(fft_mix_audio, floor(fs2/resampled_ratio), fs2);
        mix_frequency_resolution = 1/mix_time_total_length;
        mix_frequency_axis = 0:mix_frequency_resolution:(mix_audio_samples-1)*mix_frequency_resolution;
        resampled_mix_frequency_axis = resample(mix_frequency_axis, floor(fs2/resampled_ratio), fs2);
        abs_resampled_fft_mix_audio = abs(resampled_fft_mix_audio);
        plot(ax5,resampled_mix_frequency_axis,abs_resampled_fft_mix_audio);
        set(gca,'FontSize',6);
        angle_resampled_fft_mix_audio = angle(resampled_fft_mix_audio);
        plot(ax8,resampled_mix_frequency_axis,angle_resampled_fft_mix_audio);
        set(gca,'FontSize',6);
    end
    
    function show_denoised_audio(~, ~)
        resampled_ratio = 1;
        resampled_denoised_audio = resample(denoised_audio, floor(fs2/resampled_ratio), fs2);
        denoised_audio_samples = length(denoised_audio);
        Ts=1/fs2;
        denoised_time_total_length=(denoised_audio_samples-1)*Ts;
        denoised_time_axis = 0:Ts:denoised_time_total_length;
        resampled_denoised_time_axis = resample(denoised_time_axis, floor(fs2/resampled_ratio), fs2);
        plot(ax3,resampled_denoised_time_axis,resampled_denoised_audio);
        title(ax3, '降噪后音频时域波形');
        xlabel(ax3, '采样点');
        ylabel(ax3, '幅值');
        set(gca,'FontSize',6);
        fft_denoised_audio=fft(mix_audio);
        resampled_fft_denoised_audio = resample(fft_denoised_audio, floor(fs2/resampled_ratio), fs2);
        denoised_frequency_resolution = 1/denoised_time_total_length;
        denoised_frequency_axis = 0:denoised_frequency_resolution:(denoised_audio_samples-1)*denoised_frequency_resolution;
        resampled_denoised_frequency_axis = resample(denoised_frequency_axis, floor(fs2/resampled_ratio), fs2);
        abs_resampled_fft_denoised_audio = abs(resampled_fft_denoised_audio);
        plot(ax6,resampled_denoised_frequency_axis,abs_resampled_fft_denoised_audio);
        title(ax6, '幅度谱');
        xlabel(ax6, '采样点');
        ylabel(ax6, '幅值');
        set(gca,'FontSize',6);
        angle_resampled_fft_denoised_audio = angle(resampled_fft_denoised_audio);
        plot(ax9,resampled_denoised_frequency_axis,angle_resampled_fft_denoised_audio);
        title(ax9, '相位谱');
        xlabel(ax9, '采样点');
        ylabel(ax9, '幅值');
        set(gca,'FontSize',6);
    end

    function denoise_audio(~, ~)
        % 记录“主讲人音频.wav”幅度谱中幅度最大的前max_amplitudes_frequency_number个分量的序号，
        FS=fs1;
        max_amplitudes_frequency_number = floor(speaker_audio_samples/10);
        disp(max_amplitudes_frequency_number);
        % 将“主讲人音频.wav”幅度谱中的幅度值从大到小排序，并记录它们的索引位置
        [~, sorted_indexs] = sort(abs_resampled_fft_speaker_audio,'descend');
        % 创建一个新的、长度为“混合后音频.wav”样本数mix_audio_samples的零向量
        processed_fft_audio = zeros(1, mix_audio_samples) ;
        % 对于每个需要保留的频率组件的索引，将其从傅里叶变换结果
        % fft_mix_audio 中提取对应的分量，并将它们存储在新的变量中
        for temp_i=1 : 1 : max_amplitudes_frequency_number
            % 获取主讲人音频幅度谱中幅度最大的前max_amplitudes_frequency_number个频率分量的原始索引位置
            temp_index1 = sorted_indexs(temp_i);
            % 根据采样率和FFT长度计算出主讲人音频和混合音频的频率分辨率
            % 计算出新的索引位置，这里用了四舍五入来保证索引位置为整数
            temp_index2 = round( speaker_frequency_resolution / mix_frequency_resolution * temp_index1 );
            % 将混合音频中对应的频率分量拷贝到处理后音频的FFT信号的对应位置上
            processed_fft_audio(temp_index2) = fft_mix_audio(temp_index1);
        end
        processed_time_audio = ifft(processed_fft_audio);
        processed_time_audio = (real(processed_time_audio)).';
        % 对于任何小于-1或大于1的值，将其修改为-1或1
        processed_time_audio(processed_time_audio<-1) = -1;
        processed_time_audio(processed_time_audio>1) = 1;
        % 将处理后的音频信号保存到 WAV 文件中，并指定采样率和采样深度
        audiowrite('处理后音频.wav', processed_time_audio, FS, 'BitsPerSample', 8);
        % 读取混合后音频
        [denoised_audio, fs3] = audioread('处理后音频.wav');
        player3 = audioplayer(denoised_audio, fs3);
        % 创建“显示处理后”按钮和波形图
        uicontrol('Parent', f, 'Style', 'pushbutton', 'String', '显示去噪音频信息', 'Position', [50, 170, 120, 30], 'Callback', @show_denoised_audio);
    end


end

```

